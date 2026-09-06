# rushhouse-build

Build pipeline for **Rushhouse Diner** (iOS). This repository contains a GitHub Actions workflow
and, on a throwaway branch, an AES-256-CBC encrypted archive of an already-compiled Xcode project.

**There is no game source here.** Rushhouse Diner itself lives in a separate, private repository.
Nothing in this repo is redistributable, and the payload cannot be read without a key that exists
only on the developer's machine and as a repository secret.

## Why is this public?

GitHub gives macOS runners away free on public repositories only. On a private repository a single
~20-minute macOS archive bills about 200 of the 2000 free monthly minutes, because macOS carries a
10x multiplier — roughly ten iOS builds a month, total. Splitting the project in two keeps the
source closed and the runners free:

| | |
| --- | --- |
| `KaanIpek/rushhouse` | **private** — the game. Never runs a macOS job. |
| `KaanIpek/rushhouse-build` | **public** — this workflow, and ciphertext. |

## What the workflow does

Reassembles the encrypted payload from its 45 MB chunks, checks the ciphertext's md5 *before*
involving the key (so a truncated chunk reads as a truncated chunk and not as "bad decrypt"),
decrypts it, then archives, signs and uploads the result to TestFlight with `altool`.

Every run log here is world-readable, so no step ever echoes a secret.
