<script>
    import {jwtDecode} from "jwt-decode";
    import {onMount} from "svelte";

    /** @type {UserInfo|null} */
    let user = null;

    onMount(() => {
        const token = window.sessionStorage.getItem("ACC_TKN");
        if (!token) {
            alert("로그인 안돼있음요..");
            goToLogin();
            return;
        }
        const tokenInfo = jwtDecode(token);
        user = {
            name: tokenInfo.username,
            email: `${tokenInfo.studentID}@vision.hoseo.edu`,
            studentId: tokenInfo.studentID,
            userId: tokenInfo.userID,
        };
    });

    function goToLogin() {
        window.location.href = '/login';
    }

    /**
     * @typedef {{name: string, email: string, studentId: string, userId: string}} UserInfo
     */
</script>
<section>
    <header>
        <img id="logo" src="assets/logo_with_text.svg" alt="logo">
    </header>
    <div class="profile-container">
        <img class="profile-image" src="assets/logo.svg" alt="profile">

        {#if !user}
            <p>foo</p>
        {:else}
            <div class="profile-info">
                <h2>{user.name}</h2>
                <p>{user.studentId}</p>
                <p>Email: {user.email}</p>
                <div style="height: 3rem"></div>
                <p style="color: #a7a7a7; font-size: 14px">uid: {user.userId}</p>
            </div>
        {/if}
    </div>
</section>
<style lang="scss">

  section {
    padding-left: 20rem;
    padding-right: 20rem;
  }

  header {
    margin-top: 1.5rem;

    #logo {
      height: 5rem;
    }
  }

  .profile-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1.25rem;
    border-radius: 0.625rem;
    margin: 2rem auto auto;
    box-shadow: rgba(100, 100, 111, 0.2) 0 7px 29px 0;

    .profile-info {
      text-align: center;
    }

    .profile-info h2 {
      margin: 0;
      color: #333;
      font-size: 1.7rem;
    }

    .profile-info p {
      margin: 5px 0;
      color: #666;
      font-size: 1rem;
    }

    .profile-image {
      height: 6rem;
      width: 6rem;
      border: solid 2px #bcbcbc;
      border-radius: 1.5rem;
      margin-bottom : 1rem;
    }
  }
</style>
