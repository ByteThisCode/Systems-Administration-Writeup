# Examples

---------------
> [Back to Shell Scripting](../../../../tree/main/Shell_Scripting)
---------------
## - Contents

0. [Quoting](#--quoting)


---------------
## - Quoting

 * Directory file: `a a* a2 b1 b11`  
    ```  
    bash$ A=a*  
    bash$ echo $A / echo "$A" / echo '$A  
      a a* a2 / a* / $A  
    ```  
    ```  
    bash$ A=[12]  
    bash$ echo $A / echo "$A" / echo '$A'
      [12] / [12] / $A
    bash$ echo a$A / echo a"$A" / echo a'$A'
      a2 / a[12] / a$A
    ```  
    ```
    bash$ A={a1,a2}
    bash$ echo {a1,a2} / echo $A
      a1 a2 / {a1,a2} 
    ```
    ```
    bash$ mkdir f; cp * f 
      cp: -r not specified; omitting directory 'f'
    bash$ ls -l
      total 4
      -rw-r--r-- 1 las las 0 Mar 20 16:32  a
      -rw-r--r-- 1 las las 0 Mar 20 16:32 'a*'
      -rw-r--r-- 1 las las 0 Mar 20 16:32  a2
      -rw-r--r-- 1 las las 0 Mar 20 16:32  b1
      -rw-r--r-- 1 las las 0 Mar 20 16:32  b11 
    bash$ ls -l *
     -rw-r--r-- 1 las las 0 Mar 20 16:32  a 
     -rw-r--r-- 1 las las 0 Mar 20 16:32 'a*' 
     -rw-r--r-- 1 las las 0 Mar 20 16:32  a2
     -rw-r--r-- 1 las las 0 Mar 20 16:32  b1 
     -rw-r--r-- 1 las las 0 Mar 20 16:32  b11
    ```
    ```
    bash$ touch "a1 a2"
    ```
    ```
    bash$ A='impartire ls -l $A / ls -l "$A"'
    ```
