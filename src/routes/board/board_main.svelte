<style>
    .col-12 {
      width: 100%;
    }
    
    .row {
      max-width: none;
    }
    
    #page-content {
      margin-top: 2.5em;
    }
    
    a {
      color: #448bff;
      text-decoration: none;
      background-color: transparent;
    }
    
    a:hover {
      color: #005ef7;
      text-decoration: underline;
    }
    
    a:not([href]):not([tabindex]) {
      color: inherit;
      text-decoration: none;
    }
    
    a:not([href]):not([tabindex]):focus,
    a:not([href]):not([tabindex]):hover {
      color: inherit;
      text-decoration: none;
    }
    
    a:not([href]):not([tabindex]):focus {
      outline: 0;
    }
    
    .text-muted {
      color: #99a0ac !important;
    }
    
    .block {
      background: #fff;
      border-width: 0;
      border-radius: 0.25em;
      box-shadow: 0 0.0625em 0.1875em rgba(0, 0, 0, 0.05);
      margin-bottom: 1.5em;
    }
    
    .avatar {
      position: relative;
      line-height: 1;
      border-radius: 31.25em;
      white-space: nowrap;
      font-weight: 700;
      border-radius: 100%;
      display: -ms-flexbox;
      display: flex;
      -ms-flex-pack: center;
      justify-content: center;
      -ms-flex-align: center;
      align-items: center;
      -ms-flex-negative: 0;
      flex-shrink: 0;
      border-radius: 31.25em;
      box-shadow: 0 0.3125em 0.625em 0 rgba(50, 50, 50, 0.15);
    }
    
    .avatar img {
      border-radius: inherit;
      width: 100%;
    }
    
    @media (min-width: 62em) {
      .page-container {
        max-width: 50em;
        margin: 0 auto;
      }
      .page-sidenav {
        display: block !important;
      }
    }
    
    .list {
      padding-left: 0;
      padding-right: 0;
    }
    
    .list-item {
      position: relative;
      display: -ms-flexbox;
      display: flex;
      -ms-flex-direction: column;
      flex-direction: column;
      min-width: 0;
      word-wrap: break-word;
    }
    
    .list-item img {
      width: 6.25em;
      height: 6.25em;
    }
    
    .larger-item {
      height: 7.5em;
    }
    
    .list-item .item-author {
      font-size: 1.25em;
    }
    
    .list-item .item-except {
      font-size: 0.9375em;
    }
    
    .list-row .list-item {
      -ms-flex-direction: row;
      flex-direction: row;
      -ms-flex-align: center;
      align-items: center;
      padding: 0.1875em 0.125em;
    }
    
    .list-row .list-item>* {
      padding-left: 0.125em;
      padding-right: 0.125em;
    }
    
    .text-sm {
      font-size: 0.825em;
    }
    
    .h-1x {
      height: 1.25em;
      overflow: hidden;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
    }
    
    
    /* 버튼 */
    .btn-two {
      margin: 0.563em;
      color: white;
      padding: 0.938em 1.563em;
      display: inline-block;
      border: 0.063em solid rgba(0, 0, 0, 0.21);
      border-bottom-color: rgba(0, 0, 0, 0.34);
      text-shadow: 0 0.063em 0 rgba(0, 0, 0, 0.15);
      box-shadow: 
        0 0.063em 0 rgba(255, 255, 255, 0.34) inset, 
        0 0.125em 0 -0.063em rgba(0, 0, 0, 0.13), 
        0 0.188em 0 -0.063em rgba(0, 0, 0, 0.08), 
        0 0.188em 0.813em -0.063em rgba(0, 0, 0, 0.21);
    }
    
    .btn-two:active {
      top: 0.063em;
      border-color: rgba(0, 0, 0, 0.34) rgba(0, 0, 0, 0.21) rgba(0, 0, 0, 0.21);
      box-shadow: 0 0.063em 0 rgba(255, 255, 255, 0.89), 0 0.063em rgba(0, 0, 0, 0.05) inset;
      position: relative;
    }
    
    .btn-two.small {
      padding: 0.5em 1.125em;
      font-size: 0.875em;
    }
    
    .btn-3d.blue {
      background-color: #6DA2D9;
      box-shadow: 
        0 0 0 0.063em #6698cb inset,
        0 0 0 0.125em rgba(255, 255, 255, 0.15) inset,
        0 0.5em 0 0 rgba(110, 164, 219, 0.7),
        0 0.5em 0 0.063em rgba(0, 0, 0, 0.4),
        0 0.5em 0.5em 0.063em rgba(0, 0, 0, 0.5);
    }
    
    .btn-3d.blue:active {
      box-shadow: 
        0 0 0 0.063em #6191C2 inset,
        0 0 0 0.125em rgba(255, 255, 255, 0.15) inset,
        0 0 0 0.063em rgba(0, 0, 0, 0.4);
    }
    
    .btn-two.blue {
      background-color: #7fb1bf;
    }
    </style>
    
<section>
    <div class="page-content page-container" id="page-content">
        <div class="padding">
          <div class="row">
            <div class="col-12">
              <div class="list list-row block">
                <div class="list-item larger-item">
                    <div>
                        <img src="https://i.namu.wiki/i/FgPlIpSLGsre44NP7psFtRWm012t7zln8vueYbUFjJ-jDYQIR8hkH8-wd59ZzWA5oM9tyWQ_oaSChi_Vv_5kZQ.webp" class="avatar" />
                    </div>
                    <div class="flex">
                        <a class="item-author text-color" data-abc="true">포스트 제목</a>
                        <div class="item-except text-muted text-sm h-1x">
                        포스트 내용
                        </div>
                    </div>
                    </div>

                <div class="list-item larger-item">
                    <div>
                        <img src="https://i.namu.wiki/i/FgPlIpSLGsre44NP7psFtRWm012t7zln8vueYbUFjJ-jDYQIR8hkH8-wd59ZzWA5oM9tyWQ_oaSChi_Vv_5kZQ.webp" class="avatar" />
                    </div>
                    <div class="flex">
                        <a class="item-author text-color" data-abc="true">포스트 제목</a>
                        <div class="item-except text-muted text-sm h-1x">
                        포스트 내용
                        </div>
                    </div>
                    </div>
                </div>
                <a href="#" class="btn-two blue small">작성</a>
            </div>
            </div>
        </div>
    </div>
</section>