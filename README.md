# Conscrypt

## Setup

`gradle.properties`
```
org.gradle.java.installations.paths=/path/to/graalvm
```

`build.gradle.kts`
```
            val toFile =
                Paths.get("/path/to/graalvm")
                    .toFile()
```

Change both paths to a specific GraalVM instance, you want to build your native-image with.

## BUILD

```
./gradlew nativeCompile --no-daemon --rerun-tasks
```

## ERROR: native-image build

Building with GraalVM 22.3 works fine, but building with 23.0 seems to fail:

```
The build process encountered an unexpected error:

> com.oracle.svm.core.util.VMError$HostedError: com.oracle.svm.core.util.UserError$UserException: Image heap writing found a class not seen during static analysis. Did a static field or an object referenced from a static field change during native image generation? For example, a lazily initialized cache could have been initialized during image generation, in which case you need to force eager initialization of the cache before static analysis or reset the cache using a field value recomputation.
    class: sun.security.x509.X509CertImpl
  reachable through:
    object: [
[
  Version: V3
  Subject: CN=JCE Code Signing CA, OU=Java Software Code Signing, O=Oracle Corporation
  Signature Algorithm: SHA256withRSA, OID = 1.2.840.113549.1.1.11

  Key:  Sun RSA public key, 2048 bits
  params: null
  modulus: 17055452969822851135992683666979143033468417516051484609876299349676029392164762036831549416422250468242904965674236843772803293403274674367639000027597291165980354342488221481381151536971801625301661816742720339832977615327173058869536881152822978061111237825033757212550660110223240010192057280048506749054109628485658743646473290297361365307075301733147631825944967519762063418196630025286099013201124619373211156022657615904486255757509163698183415752543095397597179195207723060703818084876590178940763595648511724817319040693751004044340989968966872349969442926398888123828596921098899758812235847574184576447233
  public exponent: 65537
  Validity: [From: Thu Jul 07 01:48:44 CEST 2016,
               To: Tue Dec 31 01:00:00 CET 2030]
  Issuer: CN=JCE Code Signing CA, OU=Java Software Code Signing, O=Oracle Corporation
  SerialNumber: [    3c9eb1fc 89f733d3]

Certificate Extensions: 4
[1]: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
KeyIdentifier [
0000: 59 D5 01 84 D3 02 3B 8D   41 97 46 97 E7 A5 51 07  Y.....;.A.F...Q.
0010: BC 6C 20 2B                                        .l +
]
]

[2]: ObjectId: 2.5.29.19 Criticality=true
BasicConstraints:[
  CA:true
  PathLen:0
]

[3]: ObjectId: 2.5.29.15 Criticality=true
KeyUsage [
  DigitalSignature
  Key_CertSign
  Crl_Sign
]

[4]: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 59 D5 01 84 D3 02 3B 8D   41 97 46 97 E7 A5 51 07  Y.....;.A.F...Q.
0010: BC 6C 20 2B                                        .l +
]
]

]
  Algorithm: [SHA256withRSA]
  Signature:
0000: 3F 13 7F D7 C2 40 E8 1E   CE 79 86 10 6B 92 B7 FB  ?....@...y..k...
0010: 6B D5 C0 56 0C 11 3A 84   CC 28 97 BE C8 96 A0 6C  k..V..:..(.....l
0020: 7A 7B B2 D2 62 23 25 A4   24 30 0E 86 32 CF 69 EE  z...b#%.$0..2.i.
0030: 50 1D DB 6F 51 00 D6 8D   14 89 AC AF FF 3B 93 18  P..oQ........;..
0040: B8 E3 A1 AD 70 B5 3C 79   1B EE 22 83 A5 7E 6A 19  ....p.<y.."...j.
0050: 7F 57 A6 C6 4D 3E 9D 86   80 DE 54 C5 00 A5 B9 6D  .W..M>....T....m
0060: 3C A2 7C 79 B4 98 35 1A   19 1D B8 E4 21 E8 0D D6  <..y..5.....!...
0070: 62 40 F6 11 3E 3A 97 ED   D8 FF 6B 69 3F CC 24 9C  b@..>:....ki?.$.
0080: 63 75 51 8F FD DE A9 2C   14 14 BB FB 50 EF FF 1F  cuQ....,....P...
0090: B0 42 2D 17 34 A3 14 43   94 C2 C5 C3 F6 4C 3C CD  .B-.4..C.....L<.
00A0: 35 4E 16 28 28 DF A6 43   B4 CB 8C 30 76 AC F8 5B  5N.((..C...0v..[
00B0: 6A BE 31 00 1D 43 79 5A   27 0E D8 7C 1E 50 53 4E  j.1..CyZ'....PSN
00C0: CA 26 68 26 41 9D C9 C5   BF 55 70 45 F7 57 B5 15  .&h&A....UpE.W..
00D0: B8 31 6F 79 DA 3D B6 7F   39 48 AF 95 44 E8 16 32  .1oy.=..9H..D..2
00E0: 02 48 B0 E5 AB C4 F1 11   A3 61 21 0E C1 D4 9C 70  .H.......a!....p
00F0: C6 75 B7 BD 6A 2B 46 61   5E 94 C8 38 69 03 5A C0  .u..j+Fa^..8i.Z.

]  of class: sun.security.x509.X509CertImpl
    object: [Ljava.lang.Object;@49bb647f  of class: java.lang.Object[]
    object: com.oracle.svm.core.code.ImageCodeInfo@41ccb3b9  of class: com.oracle.svm.core.code.ImageCodeInfo
    root: com.oracle.svm.core.code.ImageCodeInfo.prepareCodeInfo()
```