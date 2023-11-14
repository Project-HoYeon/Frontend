<script>

    //import CollapsibleCard from "../components/CollapsibleCard.svelte";
    import autosize from "autosize";
    import {onMount} from "svelte";
    import {jwtDecode} from "jwt-decode";
    import "./styles.scss";
    import { Tabs, Tab, TabList, TabPanel } from 'svelte-tabs';
    import Chats from "../components/Chats.svelte";
    import Schedules from "../components/Schedules.svelte";

    let innerWidth = 0, innerHeight = 0;

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
    /** @type TokenInfo */
    let tokenInfo;
    /** @type number */
    let studentID = 20221000;
    /** @type string|undefined */
    let username = "로드중";

    /** @type Posts */
    let posts = [];
    /** @type Chats */
    let chats = [];
    for (let i = 97; i < 123; ++i)
        chats.push({
            name: String.fromCharCode(i),
            message: String.fromCharCode(i),
            timestamp: Date.now(),
            isSelf: false,
        });

    chats.push({
        name: "멋쟁이 고라니 (80)",
        message: "무루무루!!",
        timestamp: Date.now(),
        isSelf: true,
    });

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
    });

    /**
     * Fetch post-list from server
     * @param {string} token
     * @returns Promise
     */
    function fetchPosts(token) {
        fetch("https://server.mooner.dev/hoyeon/api/v1/posts?from=0&limit=20", {
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

    /**
     * @typedef {Post[]} Posts
     *
     * @typedef {{id: number, title: string, content: string, isAnonymous: boolean, writtenAt: number}} Post
     *
     * @typedef {Chat[]} Chats
     *
     * @typedef {{name: string, message: string, timestamp: number, isSelf: boolean}} Chat
     *
     * @typedef {{studentID: number, username: string, userID: string, fgp: string}} TokenInfo
     */
</script>
<svelte:window bind:innerWidth bind:innerHeight/>
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
                <hr>
                <p>
                    <i class="fa-regular fa-message"></i>
                    커뮤니티
                </p>
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
                <div class="chat-list">
                    {#each chats as chat}
                        <div class="chat-wrapper" class:chat-self={chat.isSelf}>
                            <p class="chat-content" class:chat-self={chat.isSelf}>{chat.message}</p>
                            <p class="chat-sender">{chat.name}<span>&nbsp;· {formatTimestamp(chat.timestamp)}</span></p>
                        </div>
                    {/each}
                </div>
                <div class="chat-input">
                    <textarea bind:this={chatTextArea} placeholder="멋쟁이 고라니 (80) (으)로 전송됩니다..."/>
                    <button><i class="fa-solid fa-paper-plane"></i></button>
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
                    <h2>미구현</h2>
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