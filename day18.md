# day 18

* Obfuscation is the practice of making data hard to read and analyze.

* Detecting Patterns
    - The good thing about well-known obfuscation techniques is that it's easy to reverse it once you figure out the technique used.

    - You can use these quick visual clues to guess the obfuscation technique used:

        1. ROT1 - common words look “one letter off”, spaces stay the same. Easy enough to detect.
        2. ROT13 - Look for three-letter words. Common ones like  the become gur. And and becomes naq. spaces stay the same.
        3. Base64 - Long strings containing mostly alphanumeric characters (i.e., A-Z, a-z, 0–9), sometimes with + or /, often ending in = or ==.
        4. XOR - A bit more tricky. Looks like random symbols but stays the same length as the original. If a short secret was reused, you may notice a tiny repeat every few characters.
        5. gzip...

* Obfuscation/Deobfuscation tools:
    - CyberChef: also includes an operation called **Magic** that automatically guesses and tries common decoders for you. To use it, just add the Magic operation and look at the results. It will display multiple results and it's up to you to check which one ends up making sense. You can even check "Intensive mode" to make sure it tests more possibilities before giving up.

* Layered Obfuscation (multiple techniques)