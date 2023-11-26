<script>
    import "./styles.scss";

    let showPassword = false;
    /** @type HTMLFormElement */
    let form;

    function togglePWVisibility() {
        showPassword = !showPassword;
    }

    /** @this {HTMLButtonElement} */
    function tryLogin() {
        const btn = this;
        if (btn.classList.contains("is-loading"))
            return;
        const formData = new FormData(form);

        btn.classList.add("is-loading");
        fetch("https://server.mooner.dev/hoyeon/api/v1/auth/login", {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            credentials: 'include',
            body: JSON.stringify({
                'id': formData.get("id"),
                'password': formData.get("password"),
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
                    if (resp.code === 401) // Unauthorized
                        alert("계정이 존재하지 않거나 ID/비밀번호가 일치하지 않아요.");
                    else
                        throw Error(`HttpStatus ${resp.code}: ${resp.message}`);
                } else {
                    window.sessionStorage.setItem("ACC_TKN", resp.token);
                    window.location.href = "/hoyeon";
                }
            })
            .catch(e => {
                alert("서버와의 통신에 실패했어요. 잠시 후 다시 시도해주세요.");
                console.error(e);
            })
            .finally(() => {
                btn.classList.remove("is-loading");
            });
    }
</script>
<section>
    <img id="bg-mask" src="assets/login_bg_mask.png" alt="background mask">
    <div>
        <div class="login_container">
            <hr>
            <p id="title">로그인</p>
            <p id="desc">호연 서비스에 접근하려면 로그인이 필요합니다.</p>
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
                <button type="button" class="c-pointer" on:click={tryLogin}>
                    <span><i class="fa-solid fa-right-to-bracket"></i>&nbsp;&nbsp;로그인</span>
                </button>
            </form>
            <div id="reg-wrapper" class="c-pointer">
                <hr><span on:click={()=>{ window.location.href = "/hoyeon/register" }}>회원가입</span><hr>
            </div>
            <div id="forgot-pw-wrapper" class="c-pointer">
                <a>비번을 몰?루</a>
            </div>
        </div>
    </div>
</section>