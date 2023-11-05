

<div class="login-container">
    <div class="logo">
        <svg xmlns="http://www.w3.org/2000/svg" width="212" height="212" viewBox="0 0 212 212" fill="none">
            <path d="M107.449 87.3672C128.152 73.2891 136.019 54.4492 134.984 39.3359C177.886 62.8017 152.375 89.4375 130.016 105.793C107.656 122.148 88.0627 143.452 94.6132 172.043C78.0143 165.449 76.6015 129.395 71.8398 122.355C67.0781 115.316 51.3437 106.414 42.0273 111.176C40.9922 100.203 49.8945 101.066 51.3437 101.066C59.2109 82.114 86.746 101.445 107.449 87.3672Z" fill="#214EB3"/>
            <path d="M63.5586 103.93C61.3839 104.201 59.2109 101.066 59.2109 101.066L70.1836 99.582C70.1836 99.582 66.8711 103.516 63.5586 103.93Z" fill="white"/>
            <path d="M146.992 138.711C130.016 147.406 110.969 148.441 96.6837 172.871C93.69 155.273 103.309 131.879 115.524 124.426C127.738 116.973 158.172 112.832 170.594 79.707C180.738 116.973 163.969 130.016 146.992 138.711Z" fill="#05A2AB"/>
        </svg>
      <p>로그인 창</p>
    </div>
    <div class ="form-div">
        <h3 class="form-h3">로그인</h3>
        <form method ="POST" action ="/login"class="login-form">
          <input type="text" id="username" name="username" placeholder="학번">
          <input type="password" id="password" name="password" placeholder="비밀번호">
          <button type="submit">로그인</button>
          <button type="button" class="signup-btn">회원가입</button>
        </form>
    </div>
  </div>
  
  
  <style>
    /* 모든 요소에 box-sizing: border-box; 적용 */
    *, *::before, *::after {
        box-sizing: border-box;
    }

    body {
        font-family: 'Arial', sans-serif;
        margin: 0;
        padding: 0;
    }

    .login-container {
        background-color: white;
        display: flex;
        width: 100vw;  
        height: 100vh; 
    }

    .logo {
        text-align: center;
        flex-basis: 25%;
        background-color: #E0E6F5;
    }
 
    .form-div {
        display: flex;
        flex-basis: 75%;
        flex-direction: column;
        justify-content: center; 
        align-items: center; 
        padding : 20px;
    }
    .form-h3{
        color: #000;
    font-family: NanumSquare Neo;
    font-size: 3rem;
    font-style: normal;
    font-weight: 700;
    line-height: normal;
    }

    .login-form input[type="text"],
    .login-form input[type="password"] {
        width: 100%;
        padding: 0.5rem;
        margin-bottom: 1rem;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    .login-form button {
        width: 100%;
        padding: 0.5rem;
        border: none;
        border-radius: 4px;
        color: white;
        background-color: #214EB3;
        cursor: pointer;
    }

    .login-form button:hover {
        background-color: #1e74d0;
    }

    .login-form .signup-btn {
        background-color: #05A2AB;
        margin-top: 1rem;
    }

    .login-form .signup-btn:hover {
        background-color: #39b8c8;
    }
</style>
    
  