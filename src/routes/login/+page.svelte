
        <div class="welcome-screen">
            <div class="div">
                <div class="left-panel">
                    <div class="overlap-group">
                        <div class="overlap">
                            <div class="text-wrapper">호연</div>
                            <svg xmlns="http://www.w3.org/2000/svg" width="212" height="212" viewBox="0 0 212 212" fill="none">
                                <path d="M107.449 87.3672C128.152 73.2891 136.019 54.4492 134.984 39.3359C177.886 62.8017 152.375 89.4375 130.016 105.793C107.656 122.148 88.0627 143.452 94.6132 172.043C78.0143 165.449 76.6015 129.395 71.8398 122.355C67.0781 115.316 51.3437 106.414 42.0273 111.176C40.9922 100.203 49.8945 101.066 51.3437 101.066C59.2109 82.114 86.746 101.445 107.449 87.3672Z" fill="#214EB3"/>
                                <path d="M63.5586 103.93C61.3839 104.201 59.2109 101.066 59.2109 101.066L70.1836 99.582C70.1836 99.582 66.8711 103.516 63.5586 103.93Z" fill="white"/>
                                <path d="M146.992 138.711C130.016 147.406 110.969 148.441 96.6837 172.871C93.69 155.273 103.309 131.879 115.524 124.426C127.738 116.973 158.172 112.832 170.594 79.707C180.738 116.973 163.969 130.016 146.992 138.711Z" fill="#05A2AB"/>
                            </svg>
                        </div>
                        <p class="p">호연에서 학교 생활의 모든 것을 경험해보세요.</p>
                    </div>
                </div>
                <form method ="POST" action ="/login" class="login-panel">
                    <div class="ID-button">
                        <input class="div-wrapper" placeholder=" 학번">
                    </div>
                    <div class="PW-button">
                        <input class="div-wrapper" placeholder=" 비밀번호">
                    </div>
                    <button class="login-button overlap-2">
                            <div class="text-wrapper-3">로그인</div>
                            <svg class="login" xmlns="http://www.w3.org/2000/svg" width="34" height="34" viewBox="0 0 34 34" fill="none">
                                <g clip-path="url(#clip0_85_57)">
                                  <path d="M14.5917 10.9083C14.0392 11.4608 14.0392 12.3392 14.5917 12.8917L17.2834 15.5833H4.25004C3.47087 15.5833 2.83337 16.2208 2.83337 17C2.83337 17.7792 3.47087 18.4167 4.25004 18.4167H17.2834L14.5917 21.1083C14.0392 21.6608 14.0392 22.5392 14.5917 23.0917C15.1442 23.6442 16.0225 23.6442 16.575 23.0917L21.6609 18.0058C22.2134 17.4533 22.2134 16.5608 21.6609 16.0083L16.575 10.9083C16.0225 10.3558 15.1442 10.3558 14.5917 10.9083ZM28.3334 26.9167H18.4167C17.6375 26.9167 17 27.5542 17 28.3333C17 29.1125 17.6375 29.75 18.4167 29.75H28.3334C29.8917 29.75 31.1667 28.475 31.1667 26.9167V7.08333C31.1667 5.525 29.8917 4.25 28.3334 4.25H18.4167C17.6375 4.25 17 4.8875 17 5.66667C17 6.44583 17.6375 7.08333 18.4167 7.08333H28.3334V26.9167Z" fill="white"/>
                                </g>
                                <defs>
                                  <clipPath id="clip0_85_57">
                                    <rect width="34" height="34" fill="white"/>
                                  </clipPath>
                                </defs>
                              </svg>
                    </button>
                    <div class="reg-button">
                        <button class="overlap-3">
                            <div class="text-wrapper-4">회원가입</div>
                            <svg class="add" xmlns="http://www.w3.org/2000/svg" width="34" height="34" viewBox="0 0 34 34" fill="none">
                                <g clip-path="url(#clip0_85_78)">
                                  <path d="M25.5 18.4167H18.4167V25.5C18.4167 26.2792 17.7792 26.9167 17 26.9167C16.2209 26.9167 15.5834 26.2792 15.5834 25.5V18.4167H8.50004C7.72087 18.4167 7.08337 17.7792 7.08337 17C7.08337 16.2209 7.72087 15.5834 8.50004 15.5834H15.5834V8.50004C15.5834 7.72087 16.2209 7.08337 17 7.08337C17.7792 7.08337 18.4167 7.72087 18.4167 8.50004V15.5834H25.5C26.2792 15.5834 26.9167 16.2209 26.9167 17C26.9167 17.7792 26.2792 18.4167 25.5 18.4167Z" fill="white"/>
                                </g>
                                <defs>
                                  <clipPath id="clip0_85_78">
                                    <rect width="34" height="34" fill="white"/>
                                  </clipPath>
                                </defs>
                              </svg> 
                        </button>
                    </div>
                    <div class="text-wrapper-5">로그인</div>
                    <img class="line" src="img/line-3.svg" />
                    <p class="text-wrapper-6">호연 서비스에 접근하려면 로그인이 필요합니다.</p>
                </form>
            </div>
        </div>

<style>
    .welcome-screen {
    background-color: #ffffff;
    display: flex;
    flex-direction: row;
    justify-content: center;
    width: 100%;
}

.welcome-screen .div {
    background-color: #ffffff;
    width: 1920px;
    height: 1080px;
    position: relative;
}

.welcome-screen .left-panel {
    position: absolute;
    width: 644px;
    height: 1080px;
    top: 0;
    left: 0;
}

.welcome-screen .overlap-group {
    position: relative;
    width: 640px;
    height: 1080px;
    background-color: #e0e6f5;
}

.welcome-screen .overlap {
    position: absolute;
    width: 255px;
    height: 212px;
    top: 301px;
    left: 135px;
}

.welcome-screen .text-wrapper {
    width: 139px;
    top: 132px;
    left: 116px;
    font-family: "Noto Sans KR-Medium", Helvetica;
    font-weight: 500;
    color: #000000;
    font-size: 56px;
    text-align: center;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
}

.welcome-screen .frame {
    position: absolute;
    width: 212px;
    height: 212px;
    top: 0;
    left: 0;
}

.welcome-screen .p {
    width: 413px;
    top: 531px;
    left: 110px;
    font-family: "NanumSquare Neo-Bold", Helvetica;
    font-weight: 700;
    color: #000000;
    font-size: 24px;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
}

.welcome-screen .login-panel {
    position: absolute;
    width: 511px;
    height: 507px;
    top: 286px;
    left: 1026px;
}

.welcome-screen .ID-button {
    position: absolute;
    width: 509px;
    height: 77px;
    top: 136px;
    left: 0;
}

.welcome-screen .div-wrapper {
    position: relative;
    width: 507px;
    height: 77px;
    background-color: #f7f7f7;
    border-radius: 14px;
    border: 3px solid;
    border-color: #cfcfcf;
    font-family: "NanumSquare Neo-Regular", Helvetica;
    font-weight: 400;
    color: #070404;
    font-size: 28px;
    white-space: nowrap;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
    padding-left: 20px
}
input::placeholder {
    font-family: "NanumSquare Neo-Regular", Helvetica;
    font-weight: 400;
    color: #b3b3b3;
    font-size: 28px;
    white-space: nowrap;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
    opacity: 1;

}


.welcome-screen .text-wrapper-2 {
    width: 171px;
    top: 21px;
    left: 27px;
    font-family: "NanumSquare Neo-Regular", Helvetica;
    font-weight: 400;
    color: #b3b3b3;
    font-size: 28px;
    white-space: nowrap;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
    
}

.welcome-screen .PW-button {
    position: absolute;
    width: 509px;
    height: 77px;
    top: 232px;
    left: 0;
    opacity: 1;
    transition: transform 0.3s ease, box-shadow 0.3s ease
}
.login-button:hover {
    transform: scale(1.01); /* 확대 효과 */
    box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.2); /* 그림자 효과 */

}

.welcome-screen .login-button {
    position: absolute;
    width: 509px;
    height: 77px;
    top: 342px;
    left: 0;
    opacity: 1;
    transition: transform 0.3s ease, box-shadow 0.3s ease
}
.login-button:hover {
    transform: scale(1.01); /* 확대 효과 */
    box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.2); /* 그림자 효과 */
    
}

.welcome-screen .overlap-2 {
    position: relative;
    width: 507px;
    height: 77px;
    background-color: #214eb3;
    border-radius: 14px;
}

.welcome-screen .text-wrapper-3 {
    width: 95px;
    top: 23px;
    left: 223px;
    font-family: "NanumSquare Neo-Bold", Helvetica;
    font-weight: 700;
    color: #ffffff;
    font-size: 28px;
    text-align: center;
    white-space: nowrap;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
}

.welcome-screen .login {
    position: absolute;
    width: 34px;
    height: 34px;
    top: 22px;
    left: 189px;
}

.welcome-screen .reg-button {
    position: absolute;
    width: 509px;
    height: 77px;
    top: 430px;
    left: 0;
}

.welcome-screen .overlap-3 {
    position: relative;
    width: 507px;
    height: 77px;
    background-color: #05a2ab;
    border-radius: 14px;
    opacity: 1;
    transition: transform 0.3s ease, box-shadow 0.3s ease
}
.overlap-3:hover{
    transform: scale(1.01); /* 확대 효과 */
    box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.2); /* 그림자 효과 */
}

.welcome-screen .text-wrapper-4 {
    width: 111px;
    top: 23px;
    left: 215px;
    font-family: "NanumSquare Neo-Bold", Helvetica;
    font-weight: 700;
    color: #ffffff;
    font-size: 28px;
    text-align: center;
    white-space: nowrap;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
}

.welcome-screen .add {
    position: absolute;
    width: 34px;
    height: 34px;
    top: 21px;
    left: 181px;
}

.welcome-screen .text-wrapper-5 {
    width: 146px;
    top: 6px;
    left: 0;
    font-family: "NanumSquare Neo-Bold", Helvetica;
    font-weight: 700;
    color: #000000;
    font-size: 48px;
    white-space: nowrap;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
}

.welcome-screen .line {
    position: absolute;
    width: 26px;
    height: 4px;
    top: -4px;
    left: 5px;
}

.welcome-screen .text-wrapper-6 {
    width: 506px;
    top: 69px;
    left: 1px;
    font-family: "NanumSquare Neo-Light", Helvetica;
    font-weight: 300;
    color: #000000;
    font-size: 24px;
    position: absolute;
    letter-spacing: 0;
    line-height: normal;
}

</style>