Arvin Zhang

# Lab Report 4 - Vim

TASK: In DocSearchServer.java, change the name of the start parameter of getFiles, and all of its uses, to instead be called base.

Commands (Assuming we are already in week6-skill-demo1 directory):

```
vim<space><shift>D<tab><Enter>

:%s/start/base<Enter>

:wq<Enter>
```

Total Key Strokes: 28

### Breakdown:

```
vim<space><shift>D
```

Enter the command vim along with the first letter of the desired file. Then press tab to automatically fill out the file name. Press enter to run the command. This enters vim mode and allows the user to edit the document with vim.

![replace 1](./LabReport4Imgs/replace1.png)
