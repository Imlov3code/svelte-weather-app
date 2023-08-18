<script>
  import axios from "axios";

  const openweather_key = "c8706fba81ccb44269c2baa95d7b75e5";
  const heremap_apikey = 's-1PUSnBx8P8uXElmVQZdHqGcOgvknPjNfM-CnWJw3s';

  let loading = false;
  
  let city = "";
  let temp = "";
  let humidity = "";
  let disc = "";

  let mapurl = "";
  let zoomlevel = 14.5;
  let incomeData = null;

  const submitHandler = () => {
      loading = true;
      console.log(city);

      axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${city}&APPID=${openweather_key}&units=metric`)
        .then(data => {
          loading = false;
          console.log(data.data);
          incomeData = data.data;

          temp = incomeData.main.temp;
          humidity = incomeData.main.humidity;
          disc = incomeData.weather[0].description;
          console.log(temp + " " + humidity + " " + disc);
          mapurl = `https://image.maps.ls.hereapi.com/mia/1.6/mapview?apiKey=${heremap_apikey}&c=${incomeData.coord.lat},${incomeData.coord.lon}&w=300&h=420&u=10k`;
        })
        .catch(Error => {
          console.log(Error.response);
          loading = false;
          window.alert(Error.response.data.message); 
          city = "";
        });
  };
</script>

<style>
    .data {
      text-align: center;
    }

    .maindiv {
      text-align: center;
      margin-top: 5%;
    }

    .tabledata {
      margin-left: 10%;
    }

    .ulwrpper {
      width: 50%;
      text-align: left;
      margin-left: 38%;
    }

    .forminput {
      margin-top: 3%;
    }

    .loader {
      margin-left: 45%;
      border: 16px solid #f3f3f3;
      border-top: 16px solid #3498db;
      border-radius: 50%;
      width: 120px;
      height: 120px;
      animation: spin 2s linear infinite;
    }

    @keyframes spin {
      0% {
        transform: rotate(0deg);
      }
      100% {
        transform: rotate(360deg);
      }
    }
</style>

<div class="maindiv">
  <h1>My Weather Web App</h1>

  {#if loading}
    <div class="loader" />
  {/if}

  <form class="forminput" on:submit|preventDefault={submitHandler}>

    <input bind:value={city} placeholder="Enter your city" />

    <button>Fetch Data</button>

  </form>

  {#if incomeData !== null}
    <div class="data">

      <div class="ulwrpper">

        <table class="tabledata" style="width:40%">

          <tr>
            <td>Tempurature :</td>
            <td>
              <span>{temp}&deg;</span>
            </td>

          </tr>
          <tr>

            <td>Humidity :</td>
            <td>
              <span>{humidity}%</span>
            </td>
          </tr>
          <tr>

            <td>weather like :</td>
            <span>{disc}</span>
          </tr>
        </table>

      </div>

      <img src={mapurl} alt="mapImageView" />
    </div>
  {/if}
</div>