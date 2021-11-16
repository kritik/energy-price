
<style>
  .price{
    text-align: end;
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
    const data = res.data.ee
    console.log(data)
    let html = ""
    for (const row of data){
      const time = new Date(row.timestamp*1000);
      html += `<tr><td>${time.toLocaleString()}</td><td class="price">${row.price.toFixed(2)}</td></tr>`
    }
    prices.innerHTML = html
  })
</script>
