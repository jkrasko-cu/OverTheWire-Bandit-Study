# Level2 -> Level3: Filenames with Spaces

## The Challenge:
Some files may have spaces, which are represented a bit uniquely in Linux. 

## The Solution:
- We must use quotations wrapping the filename or backslashes followed by a space to represent each space.
<details><summary>Solution</summary>
    <pre>
    cat ./ "--filename with spaces in it--"
    cat ./ filename\ with\ spaces\ in\ it\
    </pre>
   </details>
- Note that we combine a technique from the previous level as there are not only spaces, but dashes at the start of hte filename.

## Significance
- While many files aren't named with spaces, it's important to be able to handle cases when they are. It can be an easy snag for beginners to not be able to look up/access a file with spaces in them.

## Password
<details><summary>Password</summary>
    <pre>
    MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
    </pre>
   </details>