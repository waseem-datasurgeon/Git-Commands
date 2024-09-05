# How to create a SSH Key on Mac

### $${\color{gold} 1. \space Check \space if \space an \space SSH \space Key \space Already \space Exists }$$

Open your terminal and run the following command to see if you already have an SSH key:

```bash
    ls -al ~/.ssh
```
This will list the files in your ```.ssh``` directory. Look for files named ```id_rsa``` or ```id_ed25519``` (these are common SSH key names). 
If they exist, you already have an SSH key. Otherwise, proceed to create a new one.

### $${\color{gold} 2. \space Generate \space a \space new \space SSH \space Key } $$
If you don't have an SSH key, generate a new one with the following command:

```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
```
**Here:**
```-t ed25519``` is the type of encryption we are using.

```-C``` is the contact email associated with the key we are creating.
Replace ```"your_email@example.com"``` with the email you use for your GitHub account.

**Location:**
After running the command above, it will ask where do you want to save the key. 
If you already have a key, you can create the new key in the new folder. In that case you can give the new folder name and hit enter.
If you dont have an existing key, press Enter to accept the default location ```(/Users/yourusername/.ssh/id_ed25519)```.

You can also set a passphrase if you want, or leave it empty by pressing Enter.

### $${\color{gold} 3. \space Add \space the \space SSH \space Key \space to \space the \space SSH-Agent } $$

Now that you've created ssh keys, you need to make sure your system's ssh agent knows about it. It's like a wallet that holds multiple identity cards. To do that, in the same terminal, run

#### Start the SSH agent:

```bash
    eval "$(ssh-agent -s)"
```

This should print out something like this in the terminal.
```bash
    Agent pid 12345
```

If you see this, it means that the machine's ssh agent was able to read/evaluate the ssh key.

Now, we need to put the key into the system's ssh agent. Think of it as putting the id in the wallet so that we can present it whenever it is necessary.

First make sure if the ssh-agent exists in your mac already. To see it, run
```bash
    ~.ssh/config
```
Here, ~ represent the home directory.

if the ssh-agent is not present you should see, ```zsh: no such file or directory: /Users/youruser/.ssh/config```

To make sure that the file does not exists visually, run
```bash
    ls -a
```

When you are sure that the ```.ssh/config``` is not present, run the following commad to create it.
```bash
    touch ~/.ssh/config
```

Now the file is created, edit the ssh-agent. Create the wallet to put the id
```bash
    vim ~/.ssh/config
```

To add your private key to the ssh agent type
```bash
HOST *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

**Here:**
```/.ssh/``` is the folder name
```id_ed25519``` is the file that was created in step 1.

Then, save and quit vim

press ```esc``` in your keyboard.
type: ```:wq``` to write/save and quit out of vim.

Finally add the id into the wallet. Or Add the ssh key into the file we created.
```bash
ssh-add ~/.ssh/id_ed25519
```

### $${\color{gold} 4. \space Add \space the \space SSH \space Key \space to \space GitHub } $$

You now need to add the SSH key to your GitHub account.

First, copy the public key to your clipboard:

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```
Then, follow these steps:

- Go to GitHub SSH Key Settings and log in.
- Click New SSH key or Add SSH key.
- Paste the key into the "Key" field.
- Add a title (e.g., "MacBook").
- Click Add SSH Key.

### $${\color{orange} 5. \space Test \space your \space SSH \space Connection } $$

To ensure the connection is working, run:

```bash
ssh -T git@github.com
```
You should see a success message if everything is set up correctly.

```bash
The authenticity of host 'github.com ***'......
You have successfully authenticated, but GitHub does not provide shell access.
```



