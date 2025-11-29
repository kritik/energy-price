
<style>
  .price{
    text-align: end;
  }
  .cheap{
    color: coral;
  }
  .page-header{
    padding: 0;
  }
  .day_space{
    background: aliceblue;
  }
</style>

<button onclick="getData()">Uuenda</button>
<table>
  <thead>
    <tr>
      <th>id</th>
      <th>kuupaev</th>
      <th>hind</th>
    </tr>
  </thead>
  <tbody  id="prices">
    <tr><td colspan="3">Loading...</td></tr>
  </tbody>
</table>


<script>
  function getData(){
    const today = new Date()
    const start = new Date(today)
    start.setHours(today.getHours()-1)
    const end   = new Date(today)
    end.setHours(today.getHours()+24*2)
    const prices = document.querySelector("#prices")
    prices.innerHTML = `<tr><td colspan="3">Loading...</td></tr>`

    //original https://dashboard.elering.ee/api/nps/price
    fetch(`http://vpn.krylov.ee?start=${start.toISOString()}&end=${end.toISOString()}`).then(r=>r.json()).then(res=>{
      const data  = res.data.ee
      const cheap = data.map(row=>row.price).sort((a,b)=>a-b).slice(0,5)
      window.data = data

      let html = ""
      for (let i=0;i<data.length;i++){
        const row  = data[i];
        const time = new Date(row.timestamp*1000).toLocaleString('et-EE');
        const cheapClass = cheap.includes(row.price) ? 'cheap' : ''
        if (time.includes("00:00:00")) html += `<tr><td class="day_space" colspan="3"></td></tr>` // new day
        html += `<tr><td>${i}</td><td>${time}</td><td class="price ${cheapClass}">${row.price.toFixed(2)}</td></tr>`
      }
      prices.innerHTML = html
    })
  }
  
  getData()
</script>



<!-- Yandex.Metrika counter -->
<script type="text/javascript" >
   (function(m,e,t,r,i,k,a){m[i]=m[i]||function(){(m[i].a=m[i].a||[]).push(arguments)};
   m[i].l=1*new Date();k=e.createElement(t),a=e.getElementsByTagName(t)[0],k.async=1,k.src=r,a.parentNode.insertBefore(k,a)})
   (window, document, "script", "https://mc.yandex.ru/metrika/tag.js", "ym");

   ym(86524892, "init", {
        clickmap:true,
        trackLinks:true,
        accurateTrackBounce:true
   });
</script>
<noscript><div><img src="https://mc.yandex.ru/watch/86524892" style="position:absolute; left:-9999px;" alt="" /></div></noscript>
<!-- /Yandex.Metrika counter -->
