# Really Senseless Admins

## Cryptography: 35 points

## Solve

We fired Julius, but the new guy apparently misplaced the file with our private key. All he could find was the encrypted flag and some file labeled 'params'. [flag.enc](./flag.enc) [params.txt](./params.txt)

## Hints

* The new guy apparently just treated the flag text as a [big-endian](https://en.wikipedia.org/wiki/Endianness#Big-endian) integer. Apparently, this made it easier for him to use the online tools he had found.
* Wouldn't it be nice if there were detailed [write-ups](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) about these cryptosystems?
* We found a few notes on the admin's desk. One note just had the URL of an online [RSA tool][1]. The second note had [two](https://www.rapidtables.com/convert/number/decimal-to-hex.html) [URLs](https://www.rapidtables.com/convert/number/ascii-hex-bin-dec-converter.html) on it with 'plaintext ⇒ hex ⇒ int' scribbled beside it. The third one makes it look like the plaintext generated by the RSA tool will start with 104.

## Solution

The **params.txt** file provides variables `p`, `q`, and `e` for the [RSA tool][1] mentioned in the hints. The **flag.enc** file provides the `ciphertext`. Plug these four variables into the RSA tool.
```
p = 307358449224319975132699066144222436685882807714668628477464201817335216538606596193560751355060198065536448811048921551943103744796203835167959124182339648204298793067277991267869053872654998299070591661074700578450383471717960105650861723722028277409986829696857069150953926359511066023147172389630847617379

q = 303471350829576435768013789752613658823267619303280231484359795713764099787912330836505995590300214096419069232518000760901540947812817189108075444569340080872397935399647918852082970284367536780791082192877335810606026563189972788572519668502112689819584235343191091347247602987573756813840123620489201016761

e = 94455416372378889873861614626768614690502155767292925830107378277763869064397

ciphertext = 4283114025152923911035595888340045945353291052428724077240532434083672670860535988992617521390377015776218879937870762711200498375035687818319070767693653249461036430539749109643993894506898303200536554473880823517650170090081898661469775834520219111924316868780908106071680577413708819774446796182485237871731687665923919579421700002183050914259953217414121146448765745051213311484828560940385537681826693169253257132283413751573902803452471983293888690885261714683772845040171628944457788661895185413277780784467763745596879789276379145487980979785943330069579558898427407780734421225352942660821198875248580703046
```

The result of these four variables produces this plaintext value:
``` 
104873340459054924181115292546315913092670595907443492684969085
```

Following the hint **plaintext => hex => int**, convert in reverse order using the conversion functions in the [rapidtables][2] site. Convert decimal to hex then hex to ASCII.

```
Plaintext (Decimal):
104873340459054924181115292546315913092670595907443492684969085
To Hex:
4143497B5072316D33735F54214D337A5F39636239653366347D
To ASCII:
ACI{Pr1m3s_T!M3z_9cb9e3f4}
```

[1]:https://www.cryptool.org/en/cto-highlights/rsa-step-by-step
[2]:https://www.rapidtables.com