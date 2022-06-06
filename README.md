<span id="busuanzi_container_site_pv" style='display:none'>总访问量：<span id="busuanzi_value_site_pv"></span> 次</span>
<span id="busuanzi_container_site_uv" style='display:none'>| 总访客数：<span id="busuanzi_value_site_uv"></span> 人</span>



## 说明

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
## 抖音去水印下载


<div>
    <form class="needs-validation" style="margin-top: 30px;" novalidate action="https://api.imjcker.com/spider/tiktokDownload_copy" method="get">
        <div class="row g-3">
            <div class="col-10">
                <input type="text" class="form-control" id="inputUrl" name="inputUrl" placeholder="抖音视频链接，不需要去掉多余部分内容" style="width:100%;"><button class="btn btn-primary" type="submit">Download</button>
            </div>
        </div>
    </form>
</div>



<hr>


## 文章目录










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

