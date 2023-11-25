<script>
    import autosize from "autosize";
    import {afterUpdate, onMount} from "svelte";
    import {jwtDecode} from "jwt-decode";
    import { Tabs, Tab, TabList, TabPanel } from 'svelte-tabs';
    import {CupertinoPane} from "cupertino-pane";
    import Chats from "../components/Chats.svelte";
    import Schedules from "../components/Schedules.svelte";
    import "./styles.scss";
    import Timetable from "../components/Timetable.svelte";

    const HTTP_ENDPOINT = "https://server.mooner.dev";
    const WS_ENDPOINT   = "wss://server.mooner.dev";
    //const HTTP_ENDPOINT = "http://localhost:8883";
    //const WS_ENDPOINT   = "ws://localhost:8883";

    let chatRooms = [
        {
            name: "대학본부",
            lastChat: "코로나관련 연봉 삭감 안내",
            thumbnail: "assets/logo.svg",
        },
        {
            name: "입학처",
            lastChat: "앗, 대학원생 타이어보다 싸다!",
            thumbnail: "assets/logo.svg",
        },
    ];

    /** @type HTMLTextAreaElement */
    let chatTextArea;
    /** @type HTMLElement */
    let chatListElement;
    /** @type TokenInfo */
    let tokenInfo;
    /** @type number */
    let studentID = 20221000;
    /** @type {string|undefined} */
    let username = "로드중";
    /** @type {WSHandler|null} */
    let chatSocket = null;

    /** @type {{id: number, name: string}|null} */
    let chatInfo = null;

    /** @type Posts */
    let posts = [];
    /** @type ChatList */
    let chats = [];

    onMount(() => {
        const token = window.sessionStorage.getItem("ACC_TKN");
        if (!token) {
            alert("로그인 안돼있음요..");
            goToLogin();
            return;
        }
        tokenInfo = jwtDecode(token);
        studentID = tokenInfo.studentID;
        username  = tokenInfo.username;

        autosize(chatTextArea);
        fetchPosts(token);
        fetchPrevChats(token)
            .then(() => {
                connectSocket(token);
            });
    });

    afterUpdate(() => {
        console.log("afterUpdate");
        if(chats) scrollToBottom(chatListElement);
    });

    /**
     * Fetch post-list from server
     * @param {string} token
     * @returns Promise
     */
    function fetchPosts(token) {
        fetch(`${HTTP_ENDPOINT}/hoyeon/api/v1/posts?from=0&limit=20`, {
            method: 'GET',
            credentials: 'include',
            headers: {
                'Authorization': `Bearer ${token}`
            }
        })
            .then(resp => {
                if (!resp.ok)
                    return {
                        code: resp.status,
                        message: resp.text(),
                    };
                return resp.json();
            })
            .then(resp => {
                if (!Array.isArray(resp) && resp.code) {
                    if (resp.code === 401) { // Unauthorized
                        alert("인증 정보가 만료되었습니다. 다시 로그인 해주세요.");
                        goToLogin();
                    } else
                        throw Error(`HttpStatus ${resp.code}: ${resp.message}`);
                } else {
                    return resp;
                }
            })
            .then(data => {
                posts = data;
            });
    }

    /**
     * Fetch previous chats from server
     * @param {string} token
     * @returns Promise
     */
    function fetchPrevChats(token) {
        return fetch(`${HTTP_ENDPOINT}/hoyeon/api/v1/chat`, {
            method: 'GET',
            credentials: 'include',
            headers: {
                'Authorization': `Bearer ${token}`
            }
        })
            .then(async resp => {
                if (!resp.ok)
                    return {
                        code: resp.status,
                        message: await resp.text(),
                    };
                return resp.json();
            })
            .then(resp => {
                if (!Array.isArray(resp) && resp.code) {
                    if (resp.code === 401) { // Unauthorized
                        alert("인증 정보가 만료되었습니다. 다시 로그인 해주세요.");
                        goToLogin();
                    } else
                        throw Error(`HttpStatus ${resp.code}: ${resp.message}`);
                } else {
                    return resp;
                }
            })
            .then(data => {
                chats = data.map(chat => {
                    return {
                        name: `${chat.sender.name}(${chat.sender.id})`,
                        message: chat.content,
                        timestamp: chat.ts,
                        isSelf: false
                    }
                });
            });
    }

    function connectSocket(token) {
        const handler = new WSHandler(`${WS_ENDPOINT}/hoyeon/api/v1/chat/global`, token, tokenInfo.userID);
        handler.onPacket((packet) => {
            switch (packet.type) {
                case "conn_info":
                    chatInfo = packet.user;
                    let nName = `${chatInfo.name}(${chatInfo.id})`;
                    for (let chat of chats)
                        if (chat.name === nName)
                            chat.isSelf = true;
                    chats = chats;
                    break;
                case "chat_recv":
                    pushChat({
                        name: `${packet.chat.sender.name}(${packet.chat.sender.id})`,
                        message: packet.chat.content,
                        timestamp: packet.chat.ts,
                        isSelf: false
                    });
                    break;
                case "user_join":
                    pushChat({
                        name: "무루",
                        message: "유저 들어옴: " + packet.user.name,
                        timestamp: packet.ts,
                        isSelf: false
                    });
            }
        });
        chatSocket = handler;
    }

    /** @param {string} content */
    function sendChat(content) {
        if (!chatSocket)
            return;
        chatSocket.sendPacket({
            type: "chat_sent",
            chat: {
                type: "sc",
                content: content,
            },
        });
        pushChat({
            name: `${chatInfo.name}(${chatInfo.id})`,
            message: content,
            timestamp: Date.now(),
            isSelf: true,
        });
    }

    function showNewPostDialog() {
        const myPane = new CupertinoPane(
            '.new-post-panel', // Pane container selector
            {
                parentElement: 'body', // Parent container
                backdrop: true,
                showDraggable: false,
                breaks: {
                    middle: { enabled: true, height: 700 },
                    bottom: { enabled: true, height: 700 },
                },
                events: {
                    onDrag: () => console.log('Drag event')
                }
            }
        );
        myPane.present({animate: true});
    }

    /** @param {Chat} chat */
    function pushChat(chat) {
        chats.push(chat);
        chats = chats;
    }

    async function scrollToBottom(node) {
        node.scroll({ top: node.scrollHeight, behavior: 'smooth' });
    }

    function goToLogin() {
        window.location.href = '/login';
    }

    function logout() {
        window.sessionStorage.clear();
        goToLogin();
    }

    /** @param {number} ts
     *  @returns string
     */
    function formatTimestamp(ts) {
        const date = new Date(ts);
        return date.getHours().toString().padStart(2, "0") + ":" + date.getMinutes().toString().padStart(2, "0");
    }

    class WSHandler {
        /** @type {WebSocket} */
        #socket;
        /** @type {((packet: Packet) => {})|null} */
        #onPacketCallback = null;

        /**
         * @param {string} addr
         * @param {string} token
         * @param {string} userId
         */
        constructor(addr, token, userId) {
            this.#socket = new WebSocket(addr + "?token=" + encodeURIComponent(token) + "&userId=" + userId);

            // @ts-ignore
            this.#socket.onmessage = function (event) {
                console.log(event.data);
                let packet = JSON.parse(event.data);
                if (!packet.type || !this.#onPacketCallback)
                    return;

                this.#onPacketCallback(packet);
            }.bind(this);
            this.#socket.onopen = function () {
                this.send(JSON.stringify({
                    "type": "hello",
                    "userID": userId
                }));
            }
        }

        /** @param {(packet: Packet) => any} callback */
        onPacket(callback) {
            this.#onPacketCallback = callback;
        }

        /** @param {Packet} packet */
        sendPacket(packet) {
            if (!this.#socket || this.#socket.readyState !== WebSocket.OPEN)
                return;

            packet.ts = Date.now();

            this.#socket.send(JSON.stringify(packet));
        }
    }

    /**
     * @typedef {Post[]} Posts
     *
     * @typedef {{id: number, title: string, content: string, isAnonymous: boolean, writtenAt: number}} Post
     *
     * @typedef {Chat[]} ChatList
     *
     * @typedef {{name: string, message: string, timestamp: number, isSelf: boolean}} Chat
     *
     * @typedef {{studentID: number, username: string, userID: string, fgp: string}} TokenInfo
     *
     * @typedef {Object<string, any>} Packet
     */
</script>
<section id="main">
    <header>
        <img id="logo" src="assets/logo_with_text.svg" alt="logo">
        <div id="user-info">
            <img src="assets/logo.svg" alt="profile">
            <div id="profile">
                <p id="username">{username}</p>
                <hr>
                <p id="std-id">{studentID}</p>
            </div>
            <div id="spacer"></div>
            <div id="notifications">

            </div>
            <button id="btn-info" class="button is-outlined">
                <i class="icon fa-regular fa-bookmark"></i>
                내 정보
            </button>
            <button id="btn-logout" class="button" on:click={logout}>
                <i class="icon fa-solid fa-right-from-bracket"></i>
                <span>로그아웃</span>
            </button>
        </div>
    </header>
    <div id="body-wrapper">
        <div class="body-section" id="community">
            <div class="posts">
                <div class="header">
                    <div>
                        <hr>
                        <p>
                            <i class="fa-regular fa-message"></i>
                            커뮤니티
                        </p>
                    </div>
                    <div class="spacer"></div>
                    <button class="button" on:click={showNewPostDialog}>
                        <i class="fa-solid fa-pen"></i>&nbsp;&nbsp;
                        새 글 쓰기
                    </button>
                </div>
                <div class="posts-list">
                    {#each posts as post}
                        <div style="display: flex">
                            <div style="flex: 1;">
                                <p class="post-title">{post.title}</p>
                                <p class="post-content">{post.content}</p>
                            </div>
                            <img src="assets/logo.svg" alt="thumbnail">
                        </div>
                    {/each}
                </div>
            </div>
            <div class="live-chat">
                <div class="chat-list" bind:this={chatListElement}>
                    {#each chats as chat}
                        <div class="chat-wrapper" class:chat-self={chat.isSelf}>
                            <p class="chat-content" class:chat-self={chat.isSelf}>{chat.message}</p>
                            <p class="chat-sender">{chat.name}<span>&nbsp;· {formatTimestamp(chat.timestamp)}</span></p>
                        </div>
                    {/each}
                </div>
                <div class="chat-input">
                    {#if !chatInfo}
                        <textarea bind:this={chatTextArea} placeholder="채팅 서버 접속중..." disabled/>
                    {:else}
                        <textarea bind:this={chatTextArea} placeholder="{chatInfo.name}({chatInfo.id}) (으)로 전송됩니다..."/>
                    {/if}
                    <button on:click={() => {sendChat(chatTextArea.value); chatTextArea.value = ''}}><i class="fa-solid fa-paper-plane"></i></button>
                </div>
            </div>
        </div>
        <div class="body-section" id="chats">
            <Tabs>
                <TabList>
                    <Tab>내 대화</Tab>
                    <Tab>시간표</Tab>
                    <Tab>학사일정</Tab>
                </TabList>
                <TabPanel>
                    <Chats {chatRooms}/>
                </TabPanel>

                <TabPanel>
                    <Timetable/>
                </TabPanel>

                <TabPanel>
                    <Schedules/>
                </TabPanel>
            </Tabs>
        </div>
    </div>

    <footer>
        Project-HoYeon by <a href="https://github.com/Project-HoYeon">Team-HoYeon</a> with ❤ and ☕
    </footer>
</section>
<div class="new-post-panel">
    <h2>새 게시글 작성</h2>
    <hr>
    <form>
        <input name="title" type="text" placeholder="대충 이곳에 제목 입력">
        <textarea name="content" placeholder="대충 이곳에 내용 입력"/>
        <div class="switch-wrapper">
            <input name="isAnonymous" type="checkbox" class="switch">
            <label for="switch" class="switch-label">
                <span class="onf-btn"></span>
            </label>
        </div>
    </form>
</div>