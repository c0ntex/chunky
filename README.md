Chunky is a simply bitflip fuzzer that was written in 2010. It works best on linux and is written in Ruby. 

Like all fuzzers, it attempts to find security issues with applications that parse file content in an unsafe way. As such, any file that has a suitable structure is a good target, such as PDF files, media players or any other data parsing app that may be interesting to pwn.

[FILE HEADER] [DATA] [CHUNK HEADER] [DATA] [CHUNK HEADER] [DATA]

It uses very trivial mechanisms to fuzz, and the payloads set by default are specifically tailored to media files. GDB is used to triage the crashes and search for specific items, and if found, the log and testcase is saves into the crashes folder for later analysis.

Althought it is quite simple, it has resulted in the finding of a few different security related issues. Perhaps the most significant issue was detailed in CVE-2011-2702, a Signedness vulnerability in eGlibc that resulted in arbitrary code execution via any application that dealt with files larger than 2GB in size, as well as the example in Mplayer. https://www.exploit-db.com/exploits/20167/

Other applications that it has found vulnerabilities in include:

MPlayer Media Player
Xine Media Player
Parole Media Player
Totem Meda Player
eGlibc

A dockerfuzz script is included to deploy the job in a Docker container and provides GUI access via Xephr. dockerfuzz.sh
Note that for some reason, using Docker causes some false positives, however it also improves the speed of fuzzing by hundreds of times, giving a massive gain on coverage.

In testing, it was perfectly fine to fuzz 10 different media files at once.

Enjoy.
