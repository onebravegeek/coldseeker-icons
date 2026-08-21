# The logos this project vouches for

Pictures that ColdSeeker shows for a token or a protocol **because somebody here put
them there**, as opposed to the ones its worker fetched from a token's own metadata.

That distinction is the whole point of this repository. A fetched picture is vouched for
by whoever published the token; one in here is vouched for by us, and that is a weaker
claim — so it lives somewhere a person can see every addition as a commit, review it,
and revert it. The main repository stays free of image blobs; this one carries them and
their provenance.

## Layout

    tokens/<mint address>.png          a token's logo
    programs/<program id>.png          a protocol's logo
    sources.json                       where each file came from

The **filename is the address**, exactly as it appears on chain. Nothing derives it,
nothing normalises it, and a file whose name is not a valid address is refused by the
worker rather than stored under a key nobody can look up.

`.png` is the extension by convention; the worker decides the real type from the file's
magic bytes and accepts PNG, JPEG, WebP and GIF. Anything else is refused.

## Adding one

1. Put the file in `tokens/` or `programs/`, named for the address.
2. Add a line to `sources.json` saying where the bytes came from — the URL you took it
   from, or the person who drew it. This is the only record that will answer "who put
   this here and why" in a year, because the bucket it lands in shows bytes and dates
   and nothing else.
3. Commit and push to `main`. CI uploads it and nothing else happens.

Deleting a file deletes it from the bucket on the next push.

## Limits, which the worker enforces and CI does not

- 256 KB per picture. Larger is refused with `413`.
- PNG, JPEG, WebP or GIF, by magic bytes. Anything else is `415`.
- The address must be base58 and a plausible length, or `400`.

A refused upload fails the workflow. It does not fail quietly, and it does not leave
half a picture behind: the worker writes the provenance record before the bytes, so the
worst interrupted state is a record nothing serves.

## What this is not

It is not a place to put a logo you found and liked. Every file here is this project
telling somebody, on the screen where they decide whether to sign, that this picture
belongs to that address. Take it from the protocol's own site or the token's own
publisher, write down which, and if you cannot say where it came from, do not add it —
the made mark it would replace is honest, and this would not be.
