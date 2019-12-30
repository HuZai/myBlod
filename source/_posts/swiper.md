---
title: swiper 相关问题
date: 2019-07-01
---

### 1. 滑动页内出现滚动条处理

##### 1.css 处理
    .swiper-slide 增加 overflow: auto;
    swiper-slide  下面的 内容要超出 滑动页

##### 2.js 设置处理 判断滑动是否到 最上面或者最下 其他时 阻止冒泡    
    var swiper = new Swiper('.swiper-container', {
          direction: 'vertical',
      });
      var startScroll, touchStart, touchCurrent;
      swiper.slides.on('touchstart', function (e) {
          startScroll = this.scrollTop;
          touchStart = e.targetTouches[0].pageY;
      }, true);
      swiper.slides.on('touchmove', function (e) {
          touchCurrent = e.targetTouches[0].pageY;
          var touchesDiff = touchCurrent - touchStart;
          var slide = this;
          var onlyScrolling = 
                  ( slide.scrollHeight > slide.offsetHeight ) && //allow only when slide is scrollable
                  (
                      ( touchesDiff < 0 && startScroll === 0 ) || //start from top edge to scroll bottom
                      ( touchesDiff > 0 && startScroll === ( slide.scrollHeight - slide.offsetHeight ) ) || //start from bottom edge to scroll top
                      ( startScroll > 0 && startScroll < ( slide.scrollHeight - slide.offsetHeight ) ) //start from the middle
                  );
          if (onlyScrolling) {
              e.stopPropagation();
          }
      }, true);

[参考](https://github.com/nolimits4web/Swiper/issues/1467)

    <!DOCtype html>
    <html lang="zh-CN">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1.0, user-scalable=0">
        <meta name="apple-mobile-web-app-capable" content="yes">
        <meta name="apple-mobile-web-app-status-bar-style" content="black">
        <meta name="format-detection" content="telephone=no">
        <link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/Swiper/4.5.0/css/swiper.min.css">
    
        <style>
        body, html {
            padding: 0;
            margin: 0;
            position: relative;
            height: 100%;
        }
        .swiper-container {
            width: 100%;
            height: 100%;
        }
        .swiper-slide {
            background: #f1f1f1;
            color:#000;
            text-align: center;
            overflow: auto;
            -webkit-overflow-scrolling: touch;
        }
        </style>
    </head>
    
    
    <body>
    
    <div class="swiper-container">
        <div class="swiper-wrapper">
            <div class="swiper-slide">Slide 1
    
    
            </div>
            <div class="swiper-slide">Slide 2
    
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Similique libero reprehenderit facilis. Pariatur illum repellat distinctio consectetur, minima ad aspernatur odio et obcaecati a unde, accusamus architecto minus, dicta consequatur!</p>
                <p>Illo laboriosam deserunt a quas tempore perspiciatis voluptates dignissimos sunt accusamus, quod quae pariatur impedit rerum, saepe, mollitia cum error consectetur officiis ipsum beatae ut. Eveniet, maxime provident? Mollitia, ullam.</p>
                <p>Nesciunt magni, voluptatum omnis, assumenda maiores harum vel ipsa illo nostrum a non quos optio repellat tenetur ducimus dolorem placeat! Minima cum, animi ullam asperiores qui modi perferendis, earum quis!</p>
                <p>Sint unde corporis, quia ducimus reprehenderit nostrum facilis itaque temporibus corrupti. Explicabo et illo itaque sequi placeat eos architecto, sed accusantium nostrum nihil autem impedit obcaecati voluptatum minima quidem saepe.</p>
                <p>Facilis in aliquam incidunt magni! Neque eos consequatur recusandae sint labore perspiciatis deleniti commodi beatae eveniet omnis, itaque expedita voluptates facere rerum distinctio, aperiam dolores totam doloremque corrupti vitae quibusdam!</p>
                <p>Nemo natus laudantium iusto harum vero praesentium earum rerum corporis pariatur, voluptas, doloribus ipsa vitae assumenda maxime cum magni cumque alias amet laborum libero hic. Eaque accusamus, ipsa assumenda. Dolores.</p>
                <p>Fuga minima perferendis deleniti sed repellendus totam suscipit. Aspernatur laboriosam debitis nisi harum in. Harum, ipsa consequuntur sequi natus, esse quod quaerat qui eligendi nulla iusto magnam fuga iste laudantium.</p>
                <p>In dolorum, aspernatur eius labore? Voluptates eos dolore id quidem, at asperiores facere ea impedit officiis vero aspernatur. Nobis itaque assumenda recusandae, facere a ab. Unde fugiat quisquam, libero suscipit.</p>
                <p>Facere maiores dicta, recusandae sit voluptates dolorem. Veritatis quam iure, commodi consectetur sunt, eligendi possimus cupiditate. Cumque quibusdam veritatis, autem! Eligendi odio eos libero adipisci, iste sapiente unde. Animi, ea!</p>
                <p>Asperiores, dolorum itaque odio exercitationem, nemo officia, atque ex excepturi, accusantium a repellendus? Rerum minima, numquam ipsa unde aperiam totam, delectus distinctio quos obcaecati odit, maiores ipsam inventore beatae ullam?</p>
                <p>Ut perspiciatis enim, voluptatibus laborum, maiores fugiat sequi dicta quam quos in nam distinctio sed corporis obcaecati nesciunt tenetur mollitia, rem eligendi nulla quasi saepe animi pariatur aspernatur! Tempora, provident.</p>
                <p>Non tempora dolore, laboriosam doloremque earum quibusdam, quae ipsam harum, repudiandae explicabo nihil nam aut at. Quisquam qui reiciendis illum, dicta nesciunt aliquid atque earum, odit repellendus debitis sapiente dolores.</p>
                <p>Ad, atque, sint. Nihil ex doloremque, numquam voluptatum qui, explicabo. Quisquam, esse at voluptate debitis perspiciatis velit dolorem accusamus voluptatum dicta, neque numquam sint, harum quis officiis rem autem dolore.</p>
                <p>Ab dolorem pariatur libero recusandae ratione aut nihil, sint quae ipsa. Nobis quidem sequi consequatur porro tempora suscipit debitis cum explicabo corrupti neque, magnam dicta sapiente perspiciatis accusamus! Nulla, blanditiis.</p>
                <p>Sit quas magnam placeat, modi recusandae id quod voluptatibus voluptatem. Sit, nihil, vitae. Dolorem ducimus dolores tenetur. Omnis impedit similique laboriosam, dolor unde voluptas animi iure vel. Facilis odit, reiciendis.</p>
                <p>Voluptas optio iusto autem molestias soluta maiores amet aut fuga ipsa inventore, reprehenderit quis voluptatem cum nihil aliquam rerum incidunt perferendis est accusantium labore, rem iure, necessitatibus quod voluptatum. Incidunt.</p>
                <p>Non ipsa, optio deserunt. Minus numquam omnis facere atque dolore suscipit eius expedita delectus dignissimos molestiae dicta inventore illo, quaerat beatae error recusandae quas sunt labore quae! Ex quidem, sed.</p>
                <p>Placeat veniam totam, error neque, dolore dolores, pariatur rerum voluptatibus recusandae reiciendis dicta itaque nisi ratione fugiat illum perferendis eaque. Accusamus dolorem, autem mollitia sit modi vitae numquam, perferendis facere.</p>
                <p>Voluptates quos recusandae pariatur quas beatae ad at odit, est rerum sint repudiandae adipisci sapiente ipsam doloremque corporis excepturi harum magnam eaque eveniet, fuga repellendus accusantium incidunt dolor perferendis? Suscipit.</p>
                <p>Eaque reprehenderit assumenda expedita sapiente error consequuntur ullam quisquam ab, placeat sit amet optio, ad corporis unde aliquam eius harum consequatur, quasi quae mollitia debitis aspernatur. Ipsam sed rerum perspiciatis.</p>
            </div>
            <div class="swiper-slide">Slide 3</div>
            <div class="swiper-slide">Slide 4</div>
            <div class="swiper-slide">Slide 5</div>
            <div class="swiper-slide">Slide 6</div>
            <div class="swiper-slide">Slide 7</div>
            <div class="swiper-slide">Slide 8</div>
            <div class="swiper-slide">Slide 9</div>
            <div class="swiper-slide">Slide 10</div>
        </div>
    </div>
    
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Swiper/4.5.0/js/swiper.min.js"></script>
    
    <script>
    let swiper = new Swiper('.swiper-container', {
        direction: 'vertical',
        initialSlide : 0,
        noSwiping : true
    });
    let startScroll, touchStart, touchCurrent;
    swiper.slides.on('touchstart', function (e) {
        startScroll = this.scrollTop;
        touchStart = e.targetTouches[0].pageY;
    }, true);
    swiper.slides.on('touchmove', function (e) {
        touchCurrent = e.targetTouches[0].pageY;
        let touchesDiff = touchCurrent - touchStart;
        let slide = this;
        let onlyScrolling =
            ( slide.scrollHeight > slide.offsetHeight ) &&
            (
                ( touchesDiff < 0 && startScroll === 0 ) ||
                ( touchesDiff > 0 && startScroll === ( slide.scrollHeight - slide.offsetHeight ) ) ||
                ( startScroll > 0 && startScroll < ( slide.scrollHeight - slide.offsetHeight ) )
            );
        if (onlyScrolling) {
            e.stopPropagation();
        }
    }, true);
    </script>
    </body>
    </html>

