
<span id="busuanzi_container_site_pv" style='display:none'>总访问量：<span id="busuanzi_value_site_pv"></span> 次</span>
<span id="busuanzi_container_site_uv" style='display:none'>| 总访客数：<span id="busuanzi_value_site_uv"></span> 人</span>

![公众号](_media/qrcode.jpg)![小程序](_media/mini.jpg)

<div id="repos"></div>



<script>
  new Vue({
    el: '#repos',
    created() {
         fetch('https://api.github.com/users/everydoc/repos')
                   .then(response => response.json())
                   .then(result => {
                        let prePre = '<a href="https://github.com/' ;
                        let preMid = '" target="_blank" >';
                        let prePost = '</a>';
                        let pre = '<img src="https://img.shields.io/github/stars/';
                        let post = '.svg?style=social">';
                        let repoList = result.map(repo => {
                            return {
                                url: prePre + repo.full_name + preMid + repo.name + prePost,
                                star: pre + repo.full_name + post,
                                memo: repo.description
                            }
                        });

                        let table = '|项目名称 |星星数量 |项目描述 | \n|:----|:----:|:----|\n';
                        repoList.forEach(r => {
                            table += '| ' + r.url + ' |' + r.star + ' |' + r.memo + ' \n'
                        });
                     document.getElementById('repos').innerHTML = marked.parse(table);
                   })
                   .catch(err => console.log(err));
      }
  })
</script>
