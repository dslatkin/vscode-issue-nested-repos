# VS Code Nested Git Dir Issue

My personal use case for nested repos where this bug impacts my work is in dev containers and Codespaces that I use to manage other repos, although this is easily reproducible both locally and in the containers. If I clone the repos I'm working on outside of my main repo used for the dev container or Codespace, then whenever I rebuild my work I lose whatever changes that haven't been pushed up from those other repos. So to avoid that, I use the nested repos feature of VS Code, which looks to be supported through [#37947](https://github.com/microsoft/vscode/issues/37947) and [#159291](https://github.com/microsoft/vscode/pull/159291).

## Steps to recreate

1. Clone this repo
2. Run the following commands to create a child repo:

```bash
mkdir child_repo
cd child_repo
git init
```

3. Create a file named `child_repo_file` inside the nested repo:

```bash
cat << EOF > child_repo_file
child_file_line_one
child_file_line_two
child_file_line_three
child_file_line_four
child_file_line_five
EOF
```

4. Add that file and commit (make sure you're committing to the child repo
   by using it as your working directory):

```bash
git add .
git commit -m "add child repo file"
```

5. Using VS Code, open that file and modify its contents to include a couple
   more lines:

```
child_file_line_one
child_file_line_two
FIRST_CHANGE
child_file_line_three
child_file_line_four
SECOND_CHANGE
child_file_line_five
```

6. Highlight between lines `child_file_line_two` and `child_file_line_five` to include the changes in the cursor's selection.
7. Using the VS Code command pallete (Ctrl/Cmd+Shift+P), run the `Git: Stage
   Selected Ranges` command.
8. **Notice the main issue captured in the screenshot below where the new lines have been staged twice.**
9. You can unstage the incorrect changes with no damage to the file.
10. If you right click the parent repo in the "Source Control Repositories" panel in the source control sidebar and [choose "Close Repository"](https://github.com/microsoft/vscode/assets/9263640/1c01b513-f059-4f67-a8be-91b3f28dab88), the issue no longer presents itself.

![image](https://github.com/microsoft/vscode/assets/9263640/cf07cd80-0e13-4fc4-b3f3-7cf8cdf82c09)
