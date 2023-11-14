<script>
    import "./styles.scss";

    let showPassword = false;
    /** @type HTMLFormElement */
    let form;

    function moveToLogin() {
        window.location.href = "/login";
    }

    function togglePWVisibility() {
        showPassword = !showPassword;
    }

    /** @this {HTMLButtonElement} */
    function tryRegistration() {
        const btn = this;
        if (btn.classList.contains("is-loading"))
            return;

        const formData = new FormData(form);

        const id       = formData.get("id");
        const password = formData.get("password");
        if (password !== formData.get("password-confirm")) {
            alert("비밀번호가 비밀번호 확인과 일치하지 않아요.");
            return;
        }

        if (id === null || password === null) {
            return;
        }

        btn.classList.add("is-loading");
        fetch("https://server.mooner.dev/hoyeon/api/v1/auth/register", {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            credentials: 'include',
            body: JSON.stringify({
                'id': parseInt(id.toString()),
                'password': password,
                'name': formData.get("username"),
            }),
        })
            .then(resp => {
                if (!resp.ok)
                    return {
                        code: resp.status,
                        message: resp.text(),
                    };
                return {
                    code: resp.status,
                    content: resp.json(),
                };
            })
            .then(resp => {
                switch (resp.code) {
                    case 409:
                        alert("이미 이 학번으로 가입된 계정이 있어요.");
                        break;
                    case 401:
                        alert("아직 이메일 인증이 완료되지 않았어요.");
                        break;
                    case 500:
                        alert("알 수 없는 오류가 발생했어요. 잠시 후에 다시 시도해주세요.");
                        break;
                    case 202:
                        alert("학번과 연결된 학교 이메일로 인증 메일이 전송되었어요!\n이메일의 버튼을 눌러 인증을 완료하고 가입을 다시 시도해주세요.");
                        break;
                    case 200:
                        alert("가입 완료");
                        window.location.href = "/login";
                        break;
                    default:
                        console.log(resp);
                }
            })
            .finally(() => {
                btn.classList.remove("is-loading");
            });
    }
</script>
<div class="wrapper">
    <section class="intro-panel">
        <img src="assets/logo_with_text.svg" alt="logo"/>
        <p class="title">호연 서비스에 가입하고 가장 빠른 호서의<br>소식을 알아보세요</p>
        <p class="desc">실시간 채팅부터 정보 공유, 시간표 확인까지 한번에.</p>
        <img class="preview" src="assets/MainScreen.png" alt="preview">
    </section>
    <section class="main-panel">
        <button on:click={moveToLogin}>&lt; 메인 화면으로</button>
        <div>
            <div class="login_container">
                <hr>
                <p id="title">회원가입</p>
                <p id="desc">호연 서비스에서 사용할 계정을 생성합니다.</p>
                <form bind:this={form}>
                    <div>
                        <p>ID (학번)</p>
                        <input name="id" type="text" inputmode="numeric" maxlength="12" placeholder="20201234"/>
                    </div>
                    <div>
                        <p>비밀번호</p>
                        <div id="pw-wrapper">
                            <input name="password" type={showPassword ? "text" : "password"}/>
                            <i id="pw-toggle" class="fa-solid c-pointer" class:fa-eye={showPassword} class:fa-eye-slash={!showPassword} on:click={togglePWVisibility}></i>
                        </div>
                    </div>
                    <div>
                        <p>비밀번호 확인</p>
                        <input name="password-confirm" type={showPassword ? "text" : "password"}/>
                    </div>
                    <div>
                        <p>이름 (닉네임)</p>
                        <input name="username" type="text" inputmode="numeric" maxlength="12" placeholder="기타치는 독수리"/>
                    </div>
                    <button type="button" class="c-pointer" on:click={tryRegistration}>
                        <span><i class="fa-solid fa-check"></i>&nbsp;&nbsp;회원가입</span>
                    </button>
                </form>
            </div>
        </div>
    </section>
</div>