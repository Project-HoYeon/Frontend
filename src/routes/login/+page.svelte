<script>

    export let showPassword = false;

    function togglePWVisibility() {
        showPassword = !showPassword;
    }
</script>
<section>
    <img src="assets/login_bg_mask.svg" alt="background mask">
    <section class="login_container">
        <hr>
        <p id="title">로그인</p>
        <p id="desc">호연 서비스에 접근하려면 로그인이 필요합니다.</p>
        <form>
            <div>
                <p>ID (학번)</p>
                <input name="id" type="text" inputmode="numeric" maxlength="12" placeholder="20201234"/>
            </div>
            <div>
                <p>비밀번호</p>
                <div id="pw-wrapper">
                    <input name="password" type={showPassword ? "text" : "password"}/>
                    <i id="pw-toggle" class="fa-solid" class:fa-eye={showPassword} class:fa-eye-slash={!showPassword} on:click={togglePWVisibility}></i>
                </div>
            </div>
            <button type="submit">
                <i class="fa-solid fa-right-to-bracket"></i>&nbsp;&nbsp;로그인
            </button>
        </form>
    </section>
</section>
<style lang="scss">
  @import "static/styles/common.scss";

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

  .login_container {
    background: transparent;
    position: absolute;
    right: 15%;
    top: 50%;
    transform: translateY(-50%);
    width: 25rem;
    height: 30rem;

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
        border-radius: 1.65rem;
        border: 2px solid #9DA8B2;
        background: #3b4859;
        margin-top: 3px;
        font-size: 1.1rem;
        font-weight: bold;
        box-sizing: border-box;

        input[type=text], input[type=password] {
          width: 90%;
          height: 3.1rem;
          border-radius: 1.65rem;
          border: none;
          background: #FFF;
          margin-top: 0;
          font-size: 1.1rem;
          font-weight: bold;
          box-sizing: border-box;
        }
      }

      #pw-toggle {
        font-family: "Font Awesome 6 Free", serif;
        font-size: 1.2rem;
        color: #ffffff;
        cursor: pointer;
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
  }
</style>