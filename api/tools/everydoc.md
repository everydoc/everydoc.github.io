## 接口说明

> 提供一些接口给大家使用，免费，资源有限，暂时不限流。



<script>
  new Vue({
    el: '#sidebar',
    data: {
        api: '关键字统计'
    },
    created() {
        fetch('https://pdf.imjcker.com/sys/menu')
          .then(response => response.json())
          .then(result => {
            document.getElementById('sidebar').innerHTML = marked.parse(result.data);
          })
          .catch(err => console.log(err));
      }
  })
</script>
