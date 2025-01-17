# Setup EarlyAuth

Early Auth is a function to protect the alt:V server against possible DDOS Attacks, this function only works with announce set to true.
It's working as followed: A client connects to the server over the server list, the early auth functions opens a external login page where the player can authenticate. If the authentication was successfull the login page is sending a post request with a token to the client. Then the website whitelists the IP of the client in the firewall of the server. Now the client can connect and can be identified by the gameserver over the given auth token. The auth token needs to be generated by the login page.

# Step-by-Step Tutorial

## Example values

In this tutorial following example values are used:

| Key   |             Value             |             Description             |
| ------ | :-------------------------------: | :-------------------------------: |
|   token           |   0123456789                                  |   The token for the masterlist (Needs to be requested from Masterbot via Discord)          |
|   earlyAuthUrl    |   https://login.example.com/index.html    |   The url to the external login page. |
|   authToken       |   authToken0123456789                         |   The token generated by the site.    |

## Step-by-Step Example

1. Add `announce: true` to `server.cfg`.
2. Add your token to `token: 0123456789` in `server.cfg`.
3. Add `useEarlyAuth: true` to `server.cfg`.
4. Add `earlyAuthUrl: 'https://login.example.com/index.html'` to `server.cfg`.
5. Add <span style="color:red">Function 1</span> to your login page, trigger this function and a firewall whitelist function after successfull login.
6. Add a check for the authToken to the playerConnect event eg <span style="color:red">Function 2</span>
7. Now the earlyAuth login is ready.

<span style="color:red">Function 1</span>

```html
<script>
    function setToken(token) {
        alt.emit('pushToken', token);
    }
</script>
```

<span style="color:red">Function 2</span>

```js
alt.on("playerConnect", (player) => {
     if(player.authToken != "authToken0123456789")
     {
         player.kick();
     }
});
```

## Request alt:V Name
To request the alt:V from a player, you can use the snipped below in your early auth.

```html
<script>
    alt.emit("requestPlayerName")

    alt.on("playerName", (name) => {
        //Do what ever you want with the same in earlyauth
    })
</script>
```

## Extra informations
If you want to close your early auth window, you have to use `alt.emit('closeMe')`