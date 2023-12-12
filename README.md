# POOL-in-ABAP
ABAP-POOL refere-se a uma técnica de programação no SAP ABAP (Advanced Business Application Programming) para armazenar dados compartilhados entre programas. O termo "POOL" sugere um conjunto de dados que pode ser compartilhado por diferentes programas. Vou explicar os conceitos básicos de ABAP-POOL:

1. **O que é um POOL em ABAP?**
   
   Um "POOL" em ABAP é uma área de memória compartilhada que pode ser acessada por diferentes programas no mesmo sistema SAP. O objetivo é armazenar dados que precisam ser compartilhados entre diferentes partes de um sistema.

2. **Criar um POOL em ABAP:**

   Você pode criar um POOL em ABAP usando a declaração `CREATE DATA`. Aqui está um exemplo simples:

   ```abap
   REPORT ZPOOL_EXAMPLE.

   TYPES: BEGIN OF t_employee,
            name TYPE string,
            age TYPE i,
          END OF t_employee.

   DATA: gt_employee TYPE TABLE OF t_employee,
         go_pool TYPE REF TO data.

   CREATE DATA go_pool TYPE TABLE OF t_employee.
   ASSIGN go_pool->* TO <gt_employee>.

   <gt_employee>[] = VALUE #( ( name = 'John' age = 30 )
                               ( name = 'Alice' age = 25 ) ).

   LOOP AT <gt_employee> INTO DATA(ls_employee).
     WRITE: / ls_employee-name,
            ls_employee-age.
   ENDLOOP.
   ```

   Neste exemplo, criamos um tipo de tabela `t_employee`, uma tabela interna `gt_employee`, e um POOL de dados (`go_pool`) que contém a tabela interna.

3. **Acessar dados do POOL em Outros Programas:**

   Você pode acessar dados do POOL em outros programas usando a declaração `GET POOL` e `SET POOL`. Aqui está um exemplo básico:

   ```abap
   REPORT ZPOOL_ACCESS.

   TYPES: BEGIN OF t_employee,
            name TYPE string,
            age TYPE i,
          END OF t_employee.

   DATA: gt_employee TYPE TABLE OF t_employee,
         go_pool TYPE REF TO data.

   GET POOL 'ZPOOL_EXAMPLE' ASSIGNING <go_pool>.
   ASSIGN <go_pool>-* TO <gt_employee>.

   LOOP AT <gt_employee> INTO DATA(ls_employee).
     WRITE: / ls_employee-name,
            ls_employee-age.
   ENDLOOP.
   ```

   Neste exemplo, usamos `GET POOL` para obter uma referência ao POOL criado no programa anterior (`ZPOOL_EXAMPLE`) e, em seguida, atribuímos a tabela interna do POOL à tabela interna local para acessar os dados.

Lembre-se de que o uso de POOLs em ABAP deve ser feito com cuidado, uma vez que o compartilhamento de dados entre programas pode levar a complexidade e dependências difíceis de rastrear. Utilize essa técnica quando for realmente necessário compartilhar dados entre diferentes programas.
