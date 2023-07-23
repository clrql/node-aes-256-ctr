# node-aes-256-ctr
This contains my implementation of an aes-256-ctr cipher class in node.js

```typescript
import crypto from "crypto";
/**
 * Serves to create an AES256CTR object to encrypt and decrypt strings
 */
export class AES256CTR {
  key: Buffer = crypto.randomBytes(32);
  iv: Buffer = crypto.randomBytes(16);
  ciphid: string = crypto.randomBytes(8*3/4).toString('base64url');
	
  /**
   * Changes the cipher key and iv
  */
  updatecipher() {
    this.key = crypto.randomBytes(32);
    this.iv = crypto.randomBytes(16);
    this.ciphid = crypto.randomBytes(8*3/4).toString('base64url');
  }

  /**
   * cipher a string
   * @param source : the string to cipher
   * @param sourceEncoding : the encoding of the string to cipher
   * @param encryptedEncoding : the encoding for the ciphered string
   * @returns ciphered source
   */
  cipher(
    source: string, 
    sourceEncoding: crypto.Encoding = 'utf8', 
    encryptedEncoding: crypto.Encoding = 'base64url'
  ) {
    const cipher = crypto.createCipheriv('aes-256-ctr', this.key, this.iv);
    let ciphered = cipher.update(source, sourceEncoding, encryptedEncoding);
    ciphered += cipher.final(encryptedEncoding);
      return ciphered;
    }

  /**
   * decipher a string
   * @param ciphid : the id of the cipher
   * @param ciphered : the string to decipher
   * @param cipheredEncoding : the ciphered string encoding
   * @param sourceEncoding :  the source string encoding
   * @returns deciphered source
   */
  decipher(
    ciphid: string,
    ciphered: string,
    cipheredEncoding: crypto.Encoding = 'base64url', 
    sourceEncoding: crypto.Encoding = 'utf8'
  ) {
    if (ciphid != this.ciphid) throw Error(`The cipher has changed it's keys`);
    const decipher = crypto.createDecipheriv('aes-256-ctr', this.key, this.iv);
    let deciphered = decipher.update(ciphered, cipheredEncoding, sourceEncoding);
    deciphered += decipher.final(sourceEncoding);
    return deciphered;
  }
}
```
