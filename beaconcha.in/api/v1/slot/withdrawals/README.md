const options = {method: 'GET'};

fetch('https://beaconcha.in/api/v1/slot/latest/withdrawals?apikey=%3CWnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0%3E', options)
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err));
