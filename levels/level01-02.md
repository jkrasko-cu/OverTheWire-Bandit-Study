# Level1 -> Level2: Hidden and Tricky Filenames

## The Challenge:
Some files may have certain characters that make them a bit trickier to find in a file system using Linux. This requires a small bit of know-how on top of basic file system navigation.

## The Solution:
- We must use absolute path notation. Trying to mistakenly read a file like 'cat -filename' will cause the command to misinterpret it as a command line option/flag.
<details><summary>Solution</summary>
    <pre>
    cat ./-
    </pre>
   </details>
- This removes the ambiguity of command line options/flags. Alternatively, cat -- "filename" also works.

## Significance
- In case there are files with preceding characters that don't make it easy to directly access through basic Linux knowledge (hidden files too; generally files users don't care about but developers would need to access), we need these techniques to be able to access them.

## Password
<details><summary>Password</summary>
    <pre>
    263JGJPfgU6LtdEvgfWU1XP5yac29mFx
    </pre>
   </details>