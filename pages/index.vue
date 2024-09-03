<script setup>
     const getAccessToken = async () => {
  const clientId = 'ecef11a47c23445bb4fb24cac0494057';
  const clientSecret = '328be8fd72c744b6b7ee27c6f50268dd';
  const authString = `${clientId}:${clientSecret}`;
  const authBase64 = btoa(authString);

  const response = await fetch('https://accounts.spotify.com/api/token', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Authorization': `Basic ${authBase64}`
    },
    body: 'grant_type=client_credentials'
  });

  const data = await response.json();
  return data.access_token;
};
    //je veut chercher la usique destin dans l'api spotify
    const musicFetch = async () => {
  const accessToken = await getAccessToken();

  const response = await fetch("https://api.spotify.com/v1/search?q=remaster%2520track%3ADoxy%2520artist%3AMiles%2520Davis&type=album", {
    method: "GET",
     });

  const data = await response.json();
  console.log(data);
};
//lire la musique
const playMusic = async () => {
    const accessToken = await getAccessToken();
    const response = await fetch("https://api.spotify.com/v1/me/player/play", {
        method: "PUT",
        headers: {
      "Authorization": `Bearer ${accessToken}`
    }
    });
    const data = await response.json();
    console.log(data);
};
</script>
<template>
  <leftBar />

    <playerBar />
  </template>