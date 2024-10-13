
# run this command in your local computer 

```
cat ~/.ssh/id_rsa.pub
```

#in server 

```
mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && sudo nano ~/.ssh/authorized_keys

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6y1zRXXSHlesujDlAZuaYJ7WValxdltFcteeMzQ1o6xX2jds/O2MSJAVHgtKuiBtx230ClssbT8y0DhY7C5AdRmMgjZSE9d3bePaaFBmW9e/Mki9mRGcT/L2rl9QHhhtcUP2cS9zPyyB/EP8OrNPt5QhUU+ljY2G+9yejkZLWw2HZtUkLYM5Ktsteis7E9VglBMNORjKMV7TmKdXxtVP2wIyns4DECofCLEJWevx/L7w0kf0PG8nu8uLEESWao9JaVyunRPkV8Mni27MfuJZBXgX98HJ+AkMEIc+T2av/rHn4LsnGKnrwTtxRV32+0CrISYs3kTNZcQRy4Wqt9BUb FC@LAPTOP-1EC2GAFR

```

```
chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
```

