<div id="main"></div>

<script>
  new Vue({
    el: '#main',
    data: {
        api: 'jrebel'
    },
    created() {
        fetch('https://pdf.imjcker.com/sys/menu')
                  .then(response => response.json())
                  .then(result => {
                    document.getElementById('sidebar').innerHTML = marked.parse(result.data);
                  })
                  .catch(err => console.log(err));
        fetch('https://pdf.imjcker.com/sys/doc?name=' + this.api)
                  .then(response => response.json())
                  .then(result => {
                    document.getElementById('main').innerHTML = marked.parse(result.data);
                  })
                  .catch(err => console.log(err));
      }
  })
</script>
