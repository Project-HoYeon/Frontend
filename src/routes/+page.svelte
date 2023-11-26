<script>
    import autosize from "autosize";
    import {afterUpdate, onMount} from "svelte";
    import {jwtDecode} from "jwt-decode";
    import {Tab, TabList, TabPanel, Tabs} from 'svelte-tabs';
    import {CupertinoPane} from "cupertino-pane";
    import * as marked from "marked";
    import DOMPurify from "dompurify";
    import Prism from "prismjs";
    import Chats from "../components/Chats.svelte";
    import Schedules from "../components/Schedules.svelte";
    import Timetable from "../components/Timetable.svelte";
    import "./styles.scss";
    import "./prism.css";
    import moment from "moment";

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
    /** @type {CupertinoPane|null} */
    let newPostPane = null;

    /** @type {{id: number, name: string}|null} */
    let chatInfo = null;

    /** @type Posts */
    let posts = [];
    /** @type ChatList */
    let chats = [];
    /** @type {{id: number, data: (Post|NamedPost|null)}} */
    let viewedPost = {
        id: -1,
        data: null,
    }

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

        initMarked();

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

    function initMarked() {
        const renderer = {
            code(code, infostring) {
                try {
                    return `<code class="highlighted">${Prism.highlight(code, Prism.languages[infostring], infostring)}</code>`;
                } catch (err) {
                    return false;
                }
            },
        };

        marked.use({ renderer });
    }

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
                posts = data.reverse();
            });
    }

    /**
     * Fetch single post from server
     * @param {number} postId
     */
    function fetchPost(postId) {
        viewedPost.id = postId;
        const token = window.sessionStorage.getItem("ACC_TKN");
        if (!token)
            return;

        fetch(`${HTTP_ENDPOINT}/hoyeon/api/v1/posts/${postId}`, {
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
                let parsed = marked.parse(data.content);
                data.content = DOMPurify.sanitize(parsed);
                if (data.isAnonymous)
                    data.authorName = "익명";
                viewedPost.data = data;
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
                    preprocessChats();
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

    /** @this {HTMLButtonElement}
     *  @param {Event} event
     */
    function writePost(event) {
        event.preventDefault();

        const token = window.sessionStorage.getItem("ACC_TKN");
        const form = this.closest("form");
        if (!form || !token)
            return;
        const formData = new FormData(form);
        fetch("https://server.mooner.dev/hoyeon/api/v1/posts/write", {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${token}`
            },
            credentials: 'include',
            body: JSON.stringify({
                "title": formData.get("title"),
                "content": formData.get("content"),
                "isAnonymous": formData.get("isAnonymous") === "on",
            }),
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
                if (resp.code) {
                    if (resp.code === 401) { // Unauthorized
                        alert("인증 정보가 만료되었습니다. 다시 로그인 해주세요.");
                        goToLogin();
                    } else
                        throw Error(`HttpStatus ${resp.code}: ${resp.message}`);
                } else {
                    return resp;
                }
            })
            .then(_ => {
                alert("게시글이 등록되었습니다.");
                if (newPostPane)
                    newPostPane.hide();
                fetchPosts(token);
            })
    }

    function showNewPostDialog() {
        newPostPane = new CupertinoPane(
            '.new-post-panel', // Pane container selector
            {
                parentElement: 'body', // Parent container
                backdrop: true,
                showDraggable: false,
                fitHeight: true,
                breaks: {
                    bottom: {
                        enabled: false
                    }
                },
                events: {
                    onDrag: () => console.log('Drag event')
                }
            }
        );
        newPostPane.present({animate: true});
    }

    function sharePost() {
        sendChat(`%POST_SHARE:${viewedPost.data.title.slice(0, 10)}%${viewedPost.id}%`);
    }

    /** @param {Chat} chat */
    function makePostChatBubble(chat) {
        let actualData = chat.message.slice("%POST_SHARE:".length).slice(0, -1);
        let args = actualData.split("%");
        let postTitle = args.slice(0, -1).join("%");
        let postId = parseInt(args[args.length - 1]);
        console.log(`${postTitle}(${postId})`);
        window.nFetchPost = () => { fetchPost(postId); }
        return `
        <div class="chat-wrapper${chat.isSelf ? " chat-self" : ''}">
            <p class="chat-content${chat.isSelf ? " chat-self" : ''}" style="background: #66a493">
                게시글을 공유했어요: ${postTitle}<br>
                <button class="button" style="padding: 0.4rem 0.4rem; background: white; color: black; width: 100%; margin-top: 6px" onclick="(() => {window.nFetchPost()})()">&lt; 확인하기</button>
            </p>
        </div>
        `;
    }

    function preprocessChats() {
        if (chatInfo) {
            let nName = `${chatInfo.name}(${chatInfo.id})`;
            for (let chat of chats)
                if (chat.name === nName)
                    chat.isSelf = true;
        }
    }

    /** @param {Chat} chat */
    function pushChat(chat) {
        chats.push(chat);
        chats = chats;
    }

    /** @param {HTMLElement} node */
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
        return moment(ts).format("YYYY.MM.DD. HH:mm");
    }

    /** @param {number} ts
     *  @returns string
     */
    function formatChatTS(ts) {
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
            this.#socket.onmessage = ((event) => {
                console.log(event.data);
                let packet = JSON.parse(event.data);
                if (!packet.type || !this.#onPacketCallback)
                    return;

                this.#onPacketCallback(packet);
            });
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
     * @typedef {{id: number, title: string, content: string, isAnonymous: boolean, writtenAt: number, authorId: string}} NamedPost
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
            <button on:click={() => { window.location.href = "/profile"; }} id="btn-info" class="button is-outlined">
                <i class="icon fa-regular fa-bookmark"></i>
                내 정보
            </button>
            <button id="btn-logout" class="button" on:click={logout}>
                <i class="icon fa-solid fa-right-from-bracket"></i>
                <span>로그아웃</span>
            </button>
        </div>
        <div class="weather-wrapper">
            <img class="weather" src="assets/weather.svg" alt="weather">
        </div>
    </header>
    <div id="body-wrapper">
        <div class="body-section" id="community">
            <div class="posts">
                {#if viewedPost.id === -1}
                    <div class="header">
                        <div>
                            <hr>
                            <p><i class="fa-regular fa-message"></i>
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
                            <div style="display: flex;">
                                <div style="flex: 1; cursor: pointer" on:click={() => fetchPost(post.id)}>
                                    <p class="post-title">{post.title}</p>
                                    <p class="post-content">{post.content}</p>
                                </div>
                                <!--<img src="assets/logo.svg" alt="thumbnail">-->
                            </div>
                        {/each}
                    </div>
                {:else}
                    <div class="header">
                        <div>
                            <hr>
                            <p><i class="fa-regular fa-message"></i>
                                게시글 보기
                            </p>
                        </div>
                    </div>
                    <hr style="margin-top: 14px; margin-bottom: 20px; background: #A1A1A1">
                    <p on:click={() => {viewedPost = { id: -1, data: null }}}>&lt; 목록으로 돌아가기</p>
                    <div class="post-detail">
                        {#if !viewedPost.data}
                            <div class="loader-wrapper">
                                <div class="spinner">
                                    <div></div>
                                    <div></div>
                                    <div></div>
                                    <div></div>
                                    <div></div>
                                    <div></div>
                                </div>
                                <p>글을 불러오는 중...</p>
                            </div>
                        {:else}
                            <h3>{viewedPost.data.title}</h3>
                            <p>{viewedPost.data.authorName} · {formatTimestamp(viewedPost.data.writtenAt)}</p>
                            <div class="post-content">
                                {@html viewedPost.data.content}
                            </div>
                            <button class="button btn-share" on:click={sharePost}>채팅방에 공유하기 ></button>
                        {/if}
                    </div>
                {/if}
            </div>
            <div class="live-chat">
                <div class="chat-list" bind:this={chatListElement}>
                    {#each chats as chat}
                        {#if (chat.message.startsWith("%POST_SHARE:"))}
                            {@html makePostChatBubble(chat)}
                        {:else}
                            <div class="chat-wrapper" class:chat-self={chat.isSelf}>
                                <p class="chat-content" class:chat-self={chat.isSelf}>{chat.message}</p>
                                <p class="chat-sender">{chat.name}<span>&nbsp;· {formatChatTS(chat.timestamp)}</span></p>
                            </div>
                        {/if}
                    {/each}
                </div>
                <div class="chat-input">
                    {#if !chatInfo}
                        <textarea bind:this={chatTextArea} placeholder="채팅 서버 접속중..." disabled/>
                    {:else}
                        <textarea bind:this={chatTextArea} placeholder="{chatInfo.name}({chatInfo.id}) (으)로 전송됩니다..."/>
                    {/if}
                    <button on:click={() => {
                        let content = chatTextArea.value;
                        if (!content || !content.replace(/\s+/g, ''))
                            return;
                        sendChat(chatTextArea.value);
                        chatTextArea.value = '';
                    }}><i class="fa-solid fa-paper-plane"></i></button>
                </div>
            </div>
        </div>
        <div class="body-section" id="side-tab">
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
        <h4>제목</h4>
        <input class="input" name="title" type="text" placeholder="글 제목 입력">
        <h5>글 내용</h5>
        <textarea class="input content" name="content" placeholder="글 내용 입력"/>
        <div>
            <label class="container">
                <input type="checkbox" name="isAnonymous" checked>
                <svg viewBox="0 0 64 64" height="1rem" width="1rem">
                    <path d="M 0 16 V 56 A 8 8 90 0 0 8 64 H 56 A 8 8 90 0 0 64 56 V 8 A 8 8 90 0 0 56 0 H 8 A 8 8 90 0 0 0 8 V 16 L 32 48 L 64 16 V 8 A 8 8 90 0 0 56 0 H 8 A 8 8 90 0 0 0 8 V 56 A 8 8 90 0 0 8 64 H 56 A 8 8 90 0 0 64 56 V 16" pathLength="575.0541381835938" class="path"></path>
                </svg>
                &nbsp;&nbsp;익명으로 작성
            </label>
            <div class="spacer"></div>
            <button class="button" on:click={writePost}>
                <i class="fa-solid fa-pen"></i>
                &nbsp;&nbsp;게시글 작성
            </button>
        </div>
    </form>
</div>