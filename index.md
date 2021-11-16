
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
</style>

<table>
  <thead>
    <tr>
      <td>kuupaev</td>
      <td>hind</td>
    </tr>
  </thead>
  <tbody  id="prices">
    <tr><td colspan="2">Loading...</td></tr>
  </tbody>
</table>

  
<script>
  const today = new Date()
  const start = new Date(today)
  start.setHours(today.getHours()-1)
  const end   = new Date(today)
  end.setHours(today.getHours()+24)
  const prices = document.querySelector("#prices")

  fetch(`https://dashboard.elering.ee/api/nps/price?start=${start.toISOString()}&end=${end.toISOString()}`).then(r=>r.json()).then(res=>{
    const data  = res.data.ee
    const cheap = data.map(row=>row.price).sort((a,b)=>a-b).slice(0,5)
    window.data = data

    let html = ""
    for (const row of data){
      const time = new Date(row.timestamp*1000);
      const cheapClass = cheap.includes(row.price) ? 'cheap' : ''
      html += `<tr><td>${time.toLocaleString('et-EE')}</td><td class="price ${cheapClass}">${row.price.toFixed(2)}</td></tr>`
    }
    prices.innerHTML = html
  })
</script>
