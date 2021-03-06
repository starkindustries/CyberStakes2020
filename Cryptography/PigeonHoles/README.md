
# Pigeon Holes

## Cryptography: 350 points

## Solve
Cars are just computers on wheels these days! See if you can extract the secret device key from this electronic key customization server: challenge.acictf.<span>com:45098 

[firmware_server.py](firmware_server.py)

## Hints
* The flag will be a recognizable english phrase
* The flag will consist of ascii characters, digits, and underscores (_)
* The flag will contain a bit of '1337' speak

## Solution

### WORK IN PROGRESS

Run the firmware script:
```
$ python3 firmware_server.py 
```
Choose option **1 create a new firmware**. This results in an error:
```
AttributeError: module 'Crypto.Cipher.AES' has no attribute 'MODE_GCM'. GCM Stands for Galois Counter Mode. 
```


StackOverflow suggests to check what modes are allowed: 
```
$ python3
>>> from Crypto.Cipher import AES
>>> dir(AES)
['AESCipher', 'MODE_CBC', 'MODE_CFB', 'MODE_CTR', 'MODE_ECB', 'MODE_OFB', 'MODE_OPENPGP', 'MODE_PGP', '_AES', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__revision__', '__spec__', 'block_size', 'blockalgo', 'key_size', 'new']
```

From the output above, the Crypto package does not have MODE_GCM. Install Cryptodome:
```
$ pip3 install pycryptodomex
```

Replace the import line in firmware_server.py to:
```
from Cryptodome.Cipher import AES
```

Now the firmware script works fine on the local machine.

Notice in class firmware, the `device_key` variable contains the flag contents:
```
self.device_key = open("./flag", "r").read()
```

Then the `device_key` variable is added to the `img` var, which is encoded, encrypted, and given back to the user.
```
def generate_image(self):
    img = "Rev:%s::Vin:%s::DeviceKey:%s::Name:%s::User_Title:%s::Code:%s" % (
        self.revision,
        self.vin,
        self.device_key,
        self.name,
        self.user_title,
        self.firmware_code)
```

Sample Data:
```
Sample flag:
ACI{pigeon_holes_is_awesome}

An example vin for the prompt:
VIN (ascii): JH4KA8250MC004002
VIN (hex):   0x 4a48344b41383235304d43303034303032

VIN (ascii): 00000000000000000
VIN (hex):   0x 3030303030303030303030303030303030

Sample encryption variables
nonce (ascii) = 'aa'
nonce (hex) = 0x 6161

AES key (ascii) = 'aaaaaaaaaaaaaaaa'
AES key (hex) = 0x 61616161616161616161616161616161

Hex encoded firmware using VIN, nonce, and key above:
f039ec05ef3a689898d4bc3f636326c516a0935767a5525737ba9af30a1bab36cee0c0750e7401d275e583e1843441c380123bdc85263a99e36114da9730f1b0bd7ba12d885ca0869feae347162afc082c5381da23a225ee4484ce220dbfefc85282afbd1d5024ef0df3ed52897039c10f1ca04299dd215f

Tag: 0x 9ab62e06248e8a598178732230431995 (16 bytes)        

For reading firmware (option 2):

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
1. Create New Firmware
2. Read Firmware
3. Write Firmware
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

2
Input HEX Encoded Firmware: 
\[Enter hex firmware code]
Input Encryption Nonce: 
\[Enter hex nonce]
Input Tag: 
\[Enter hex tag]

Sample output:
b'Rev:2::Vin:JH4KA8250MC004002::DeviceKey:ACI{pigeon_holes_is_awesome}::Name:V.i.r.t.u.a.l.K.E.Y::User_Title:My Car::Code:'
```

## Resources:
* https://en.wikipedia.org/wiki/Galois/Counter_Mode
* [What is GCM? Galois Counter Mode](https://youtu.be/g_eY7JXOc8U)
* [AES GCM Source code][1]

[1]:https://github.com/Legrandin/pycryptodome/blob/6fb4ca4c73d7e80f336c183dd8ed906d3c4320a2/lib/Crypto/Cipher/_mode_gcm.py#L155