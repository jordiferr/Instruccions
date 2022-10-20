# Instruccions d'ús llibreria libgcrypt

## Codi:

```c
#include <stdio.h>
#include <gcrypt.h>

int main(void)
{
    char *s = "some text";
    unsigned char *x;
    unsigned i;
    unsigned int l = gcry_md_get_algo_dlen(GCRY_MD_SHA256); /* get digest length (used later to print the result) */

    gcry_md_hd_t h;
    gcry_md_open(&h, GCRY_MD_SHA256, GCRY_MD_FLAG_SECURE); /* initialise the hash context */
    gcry_md_write(h, s, strlen(s)); /* hash some text */
    x = gcry_md_read(h, GCRY_MD_SHA256); /* get the result */

    for (i = 0; i < l; i++)
    {
        printf("%02x", x[i]); /* print the result */
    }
    printf("\n");
    return 0;
}
```

## Compilació

```
gcc <codi.c> -lcrypt <sortida>
```
