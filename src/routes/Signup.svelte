<style>
    * {
      margin: 0px;
      padding: 0px;
      text-decoration: none;
      font-family: sans-serif;
    }
    
    body {
        background-color: #f0f0f0;  
    }
    
    .joinForm {
      position: absolute;
      width: 400px;
      height: auto; 
      padding: 30px 20px; 
      background-color: #FFFFFF;
      text-align: center;
      top: 45%;
      left: 50%;
      transform: translate(-50%, -50%);
      border-radius: 15px;
      box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.3); 
    }
    
    .joinForm h2 {
      text-align: center;
      margin: 30px;
    }
    
    .textForm {
      border-bottom: 2px solid #adadad;
      margin: 30px;
      padding: 10px 10px;
    }
    
    .id,
    .pw,
    .name,
    .email {
      width: 100%;
      border: none;
      outline: none;
      color: #636e72;
      font-size: 16px;
      height: 25px;
      background: none;
    }
    
    .joinForm > input[type="submit"] {
      color: #fff;
      font-size: 16px;
      background-color: #6692FF;
      margin-top: 20px;
      width: 60%;
      height: 48px;
      padding: 0 10px;
      box-sizing: border-box;
      margin-bottom: 20px;
      border-radius: 6px;
      border: none; /* 테두리 제거 */
      outline: none; /* 포커스 테두리 제거 */
    }
    
</style>
    
    
<section>
    <form method="POST" class="joinForm">                                                                                       
        <h2>회원가입</h2>
        <div class="textForm">
            <input name="loginId" type="text" class="id" placeholder="아이디">
        </div>
        <div class="textForm">
            <input name="loginPw" type="password" class="pw" placeholder="비밀번호">
        </div>
        <div class="textForm">
            <input name="loginPwConfirm" type="password" class="pw" placeholder="비밀번호 확인">
        </div>
        <div class="textForm">
            <input name="name" type="text" class="name" placeholder="닉네임">
        </div>
        <div class="textForm">
            <input name="email" type="text" class="email" placeholder="이메일">
        </div>
        <input type="submit" value="회원가입">
    </form>
</section>