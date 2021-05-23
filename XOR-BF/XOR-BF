%include "includes/io.inc"

extern getAST
extern freeAST

section .bss
    ; La aceasta adresa, scheletul stocheaza radacina arborelui
    root: resd 1


section .text

; Traverse the tree (preorder), replace string values of nodes into numbers
; and apply operations between children according to the operator.
traverse:
    push ebp
    mov ebp, esp
    ; Go to the node, check if its a leaf
    ; If yes, jump to atoi conversion
    ; if no, it means its an operator,
    ; continue traversing the tree
    mov eax, [ebp + 8]
    mov eax, [eax]
    mov eax, [ebp + 8]
    mov eax, [eax + 4]
    cmp eax, 0
    je atoi
    jmp left
; Writes the information that is stored in EAX to where EDX is pointing (inside the tree)    
overwrite:
    mov edx, [ebp + 8]
    mov edx, [edx]
    mov [edx], eax
    jmp end
; Left traversal of the tree. [eax + 4] left node
left:
    mov eax, [ebp + 8]
    mov eax, [eax + 4]
    push eax
    call traverse
    add esp, 4
; Right traversal of the tree. [eax + 8] right node
right:
    mov eax, [ebp + 8]
    mov eax, [eax + 8]
    push eax
    call traverse
    add esp, 4
    ; Reset eax to the parent, cast it as dword and compare it, then jump to the appropirate operations
    mov eax, [ebp + 8]
    mov eax, [eax]
    cmp dword [eax], '+'
    je adding
    cmp dword [eax], '-'
    je subtracting
    cmp dword [eax], '*'
    je multiplying
    cmp dword [eax], '/'
    je divide
; Go to the left node, move the information to ebx, then to the right and add it to ebx as well
; Move it to eax for overwrite
; Jump to overwrite the operator with the value of the operations inbetween children
adding:
    mov eax, [ebp + 8]
    mov eax, [eax + 4]
    mov eax, [eax]
    mov ebx, [eax]
    mov eax, [ebp + 8]
    mov eax, [eax + 8]
    mov eax, [eax]
    add ebx, [eax]
    mov eax, ebx
    jmp overwrite
; Move the value of left to ebx and subtract the right from it
; Move it to eax for overwrite
; Jump to overwrite
subtracting:
    mov eax, [ebp + 8]
    mov eax, [eax + 4]
    mov eax, [eax]
    mov ebx, [eax]
    mov eax, [ebp + 8]
    mov eax, [eax + 8]
    mov eax, [eax]
    sub ebx, [eax]
    mov eax, ebx
    jmp overwrite
; Same as before, using imul for negative numbers
multiplying:
    mov eax, [ebp + 8]
    mov eax, [eax + 4]
    mov eax, [eax]
    mov ebx, [eax]
    mov eax, [ebp + 8]
    mov eax, [eax + 8]
    mov eax, [eax]
    imul ebx, [eax]
    mov eax, ebx
    jmp overwrite
; Same as before, cdq and idiv for negative numbers
divide:
    mov eax, [ebp + 8]
    mov eax, [eax + 4]
    mov eax, [eax]
    mov ebx, [eax]
    mov eax, [ebp + 8]
    mov eax, [eax + 8]
    mov eax, [eax]
    mov ecx, [eax]
    mov eax, ebx
    cdq
    idiv ecx
    jmp overwrite
    
; Initialize edx to point to the node, clear all other registers
; Move one byte from where edx is pointing to, check if its zero (string is parsed)
; If equal, jump to check to see if the string represented a 
; negative number (ecx would be set to one if it was)
; If not, check to see if we have the first byte as a minus.
; This works since we do dont have invalid numbers/strings in tests, e.g. 12-345
; If it is a minus, move edx by 1 byte, and give it to bl. 
; This way it skips the minus, while
; setting ecx to 1 as a flag.
; Subtract 48 to get ASCII value of numbers, 
; multiply eax (where the real number is being stored) by 10
; and add ebx to it (not bl since it would be a size mismatch)
; "check" negates the number if ecx was set
atoi:
    mov edx, [ebp +8]
    mov edx, [edx]
    xor eax, eax
    xor ecx, ecx
    xor ebx, ebx
atoiloop:
    mov bl, byte [edx]
    cmp bl, 0
    je check
    cmp bl, 45
    je negative
    sub bl, 48
    imul eax, 10
    add eax, ebx
    add edx, 1
    jmp atoiloop
negative:
    add edx, 1
    mov bl, byte [edx]
    mov ecx, 1
    jmp atoiloop
check:
    cmp ecx, 1
    jne overwrite
    neg eax
    jmp overwrite
end:
    leave
    ret


global main


main:
    mov ebp, esp; for correct debugging
    ; NU MODIFICATI
    push ebp
    mov ebp, esp
    
    ; Se citeste arborele si se scrie la adresa indicata mai sus
    call getAST
    mov [root], eax
    
    ; Implementati rezolvarea aici:
    push eax
    call traverse
    add esp, 4
    ; Show the result which is currently at the top of the tree
    mov eax, [root]
    mov eax, [eax]
    PRINT_DEC 4, [eax]
    
    ; NU MODIFICATI
    ; Se elibereaza memoria alocata pentru arbore
    push dword [root]
    call freeAST
    
    xor eax, eax
    leave
    ret