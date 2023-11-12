<script>

    import "./styles.scss";

    let showPassword = false;
    /** @type HTMLFormElement */
    let form;

    function togglePWVisibility() {
        showPassword = !showPassword;
    }

    function tryLogin() {
        const formData = new FormData(form);

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
            .then(resp => {
                if (!resp.ok)
                    return {
                        code: resp.status,
                        message: resp.text(),
                    };
                return resp.json();
            })
            .then(resp => {
                if (resp.code) {
                    if (resp.code === 401)
                        alert("계정이 존재하지 않거나 ID/비밀번호가 일치하지 않아요.");
                    else
                        throw Error(`HttpStatus ${resp.code}: ${resp.message}`);
                } else
                    alert("OK");
            })
            .catch(e => {
                alert("서버와의 통신에 실패했어요. 잠시 후 다시 시도해주세요.");
                console.error(e);
            })
    }
</script>
<section>
    <img id="bg-mask" src="assets/login_bg_mask.png" alt="background mask">
    <section class="login_container">
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
                <i class="fa-solid fa-right-to-bracket"></i>&nbsp;&nbsp;로그인
            </button>
        </form>
        <div id="reg-wrapper" class="c-pointer">
            <hr><a>회원가입</a><hr>
        </div>
        <div id="forgot-pw-wrapper" class="c-pointer">
            <a>비번을 몰?루</a>
        </div>
    </section>
</section>
<style lang="scss">
  @import "static/styles/common.scss";
  @import "static/styles/mixins.scss";

  #bg-mask {

    height: 100%;

    @include under-width(1150px) {
      margin-left: -6rem;
    }
  }

  p {
    font-style: normal;
    line-height: normal;
  }

  section {
    background: white;
    position: absolute;
    width: 100%;
    height: 100%;
  }

  .c-pointer {
    cursor: pointer;
  }

  .login_container {
    background: transparent;
    position: absolute;
    right: 15%;
    top: 50%;
    transform: translateY(-50%);
    width: 25rem;
    height: auto;

    @include under-width(1150px) {
      right: 10%;
    }

    hr {
      display: block;
      width: 20px;
      height: 4px;
      border-width: 0;
      margin-left: 5px;
      background: #96AFED;
    }

    #title {
      color: #000;
      font-size: 2.5rem;
      font-weight: 700;
      margin-top: 5px;
    }

    #desc {
      font-family: "NanumSquareNeo",serif;
      color: #000;
      font-size: 1.2rem;
      font-weight: 350;
    }

    form {
      margin-top: 1.5rem;

      p {
        color: #575757;
        font-size: 0.8rem;
        font-weight: 700;
        margin-left: 1.5rem;
        margin-top: 8px;
      }

      input[type=text], input[type=password] {
        width: 100%;
        height: 3.3rem;
        border-radius: 1.65rem;
        border: 2px solid #9DA8B2;
        background: #FFF;
        margin-top: 3px;
        padding-left: 1.5rem;
        font-size: 1.1rem;
        font-weight: bold;
        box-sizing: border-box;
      }

      #pw-wrapper {
        width: 100%;
        height: 3.3rem;
        margin-top: 3px;
        font-size: 1.1rem;
        font-weight: bold;
        box-sizing: border-box;
        display: flex;
        overflow: hidden;

        input[type=text], input[type=password] {
          width: 100%;
          height: 3.3rem;
          border-radius: 1.65rem;
          border: 2px solid #9DA8B2;
          background: #FFF;
          margin-top: 0;
          font-size: 1.1rem;
          font-weight: bold;
          box-sizing: border-box;
        }

        #pw-toggle {
          font-family: "Font Awesome 6 Free", serif;
          font-size: 1.2rem;
          color: #9DA8B2;
          margin-left: -2.5rem;
          margin-top: 1rem;
        }
      }

      button {
        width: 100%;
        height: 3.6rem;
        margin-top: 1.8rem;
        border-radius: 1.8rem;
        background: #3B4859;
        font-size: 1.2rem;

        &:hover {
          background: #586478;
        }

        i {
          font-family: "Font Awesome 6 Free", serif;
        }
      }
    }

    #reg-wrapper {
      width: 100%;
      margin-top: 2rem;
      display: flex;
      gap: 1rem;
      color: #9DA8B2;

      hr {
        width: 150px;
        height: 2px;
        background: #9DA8B2;
        margin: auto 0;
      }

      a {
        flex: 1;
        text-align: center;
      }
    }

    #forgot-pw-wrapper {
      width: 100%;
      text-align: center;
      margin-top: 2rem;

      a {
        color: #6692FF;
      }
    }
  }
</style>