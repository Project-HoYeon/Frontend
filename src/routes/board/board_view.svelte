
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous">

  
  <style>
    .main {
      display: flex;
      justify-content: center;
      padding-top: 50px;
    }
  
    .feed_box {
      background-color: white;
      width: 880px;
      margin: 10px;
      min-height: auto;
      padding-bottom: 10px;
    }
  
    .feed_img {
      width: 100%;
      object-fit: contain;
    }
  
    .feed_content {
      padding: 0px 10px;
    }
  
    .feed_like {
      padding: 0px 10px;
    }
  
    .feed_reply {
      padding: 0px 10px;
      display: flex;
      flex-direction: column;
    }
  
    .feed_txt {
      font-size: 14px;
    }
  
    span {
      padding-right: 5px;
    }
  
    .feed_name {
      padding: 10px;
      display: flex;
      align-items: center;
    }
  
    .feed_name_txt {
      font-size: 16px;
      padding: 0px 10px;
      font-weight: bold;
    }
  
    .profile_box {
      width: 50px;
      height: 50px;
      border-radius: 70%;
      overflow: hidden;
    }
  
    .profile_img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  
    .name_content {
      display: flex;
      flex-direction: column;
    }
  
    .name_content_txt {
      font-size: 12px;
      padding: 0px 10px;
      font-weight: bold;
      color: lightgray;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      width: 190px;
    }
  
    .big_profile_box {
      width: 60px;
      height: 60px;
      border-radius: 70%;
      overflow: hidden;
    }
  
    .link_txt {
      font-size: 14px;
      font-weight: bold;
      cursor: pointer;
      text-decoration: none;
    }
  
    .recommend_box {
      display: flex;
      justify-content: space-between;
      padding: 5px;
      font-size: 14px;
      font-weight: bold;
      align-items: center;
    }
  
    .comment_box {
      margin: 40px 0 0 5px;
      font-size: 12px;
      font-weight: bold;
      color: lightgray;
      display: flex;
      flex-direction: column;
    }
  
    .feed_txt b {
      font-size: 18px; 
      padding-left: 10px
    }
  </style>
  
<section>
    <div class="main">
      <div class="left_body">
        <div class="border feed_box">
          <div class="feed_name">
            <div class="profile_box">
              <img class="profile_img" src="data:image/jpeg;base64,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">
            </div>
            <span class="feed_name_txt">20181250</span>
          </div>
          <p class="feed_txt"><b>라면먹지마세요 체질이라는게바뀝니다</b></p>
          <img class="feed_img" src="https://blog.kakaocdn.net/dn/BsINL/btqBYtGFuHW/T8KuQKJ8eOhj15lNVnA1F0/img.png">
  
          <div class="feed_content">
              <p><b>11/13 20:23</b></p>  
            <p class="feed_txt">님들 라면드시지마세요 체질이라는게바뀝니다 예를들어 초식공룡과 육식공룡이잇는데요 초식공룡처럼힘약해질려면 라면을드시고 육식공룡처럼 힘쌔지고싶으시면 라면드시지마세요 합기도배운놈 따라하고 유도배운놈이겨서따라하고 복싱배운놈이겨서따라하고 잡종맛좀봐라 이얏 합기도+유도+복싱+태권도+ 아침밥 꼭먹고 학교가기ㅋㅋ아침밥머기전 물2잔ㅇㅋ10초간격으로 물1한마시고또물1한잔 20분후 식사ㄱㄱ밥먹을때 국잇어야됨 아침,점심,저녁 꼭 꼬박꼬박 챙겨드세요</p>
          </div>
        </div>
      </div>
    </div>
</section>