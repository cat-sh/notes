# Configuring git

```
git config --global user.name "Name"
git config --global user.email email@email.com
git config --global init.defaultBranch main
```

### (Optional) Using Commit GUI for commit messages

```
git config --global core.editor "flatpak run re.sonny.Commit"
gsettings set org.gnome.mutter center-new-windows true
```

## Adding SSH key to Github

Generate key pair:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Follow GitHub's instructions [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?tool=webui#adding-a-new-ssh-key-to-your-account).

Test the connection:

`ssh -T git@github.com`
