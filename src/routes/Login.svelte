<style>
    * {
        padding: 0;
        margin: 0;
        border: none;
    }
    body {
        font-size: 14px;
        font-family: 'Roboto', sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background-color: #f0f0f0; 
    }
    
    .login-wrapper {
        width: 400px;
        height: 350px;
        padding: 40px;
        box-sizing: border-box;
        background-color: #fff; 
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); 
    }
    
    .login-wrapper > h2 {
        font-size: 24px;
        margin-bottom: 20px;
    }
    
    #login-form > input {
        width: 100%;
        height: 48px;
        padding: 0 10px;
        box-sizing: border-box;
        margin-bottom: 16px;
        border-radius: 6px;
        background-color: #F8F8F8;
    }
    
    #login-form > input::placeholder {
        color: #D2D2D2;
    }
    
    #login-form > input[type="submit"] {
        color: #fff;
        font-size: 16px;
        background-color: #6692FF;
        margin-top: 20px;
    }

    #login-form > label {
        color: #999999;
    }

</style>

<section>
    <body>
        <div class="login-wrapper">
            <h2>로그인</h2>
            <form method="post" id="login-form">
                <input type="text" name="userName" placeholder="아이디">
                <input type="password" name="userPassword" placeholder="비밀번호">
                <input type="submit" value="로그인">
            </form>
        </div>
    </body>
</section>

