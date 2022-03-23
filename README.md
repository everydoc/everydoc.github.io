
<span id="busuanzi_container_site_pv" style='display:none'>总访问量：<span id="busuanzi_value_site_pv"></span> 次</span>
<span id="busuanzi_container_site_uv" style='display:none'>| 总访客数：<span id="busuanzi_value_site_uv"></span> 人</span>

![公众号](_media/qrcode.jpg)![小程序](_media/mini.jpg)

<div id="repos"></div>



<script>
  new Vue({
    el: '#sidebar',
    created() {
        fetch('https://imjcker.com:1997/sys/menu')
          .then(response => response.json())
          .then(result => {
            document.getElementById('sidebar').innerHTML = marked.parse(result.data);
          })
          .catch(err => console.log(err));
         fetch('https://imjcker.com:1997/sys/repos')
                   .then(response => response.json())
                   .then(result => {
                     document.getElementById('repos').innerHTML = marked.parse(result.data);
                   })
                   .catch(err => console.log(err));
      }
  })
</script>
