## Porting HTML App to CouchApp

Prerequisites:
* [Couchapp](#Couchapp installation)
* couchdb instance
* desired javascript/html application


### Couchapp installation
Clone the [couchapp repository](https://github.com/couchapp/couchapp.git), build and install it. With the following commands from a terminal:

```
git clone https://github.com/couchapp/couchapp.git 
cd couchapp
python setup.py build
sudo python setup.py install
```

### Couchdb instance
For this we will be using a pre-configured virtual machine with `couchdb instance`

Requirements:

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrantup](https://www.vagrantup.com/downloads.html)
* [Vagrant config repo](https://github.com/dogi/ole--vagrant-bells)

Once you have installed the Virtual Box and Vagrantup. You will have to clone the Vagrant config repo like so:

```
git clone https://github.com/dogi/ole--vagrant-bells/ vagrant-vm
cd vagrant-vm/release
```

In the file `Vagrantfile`, You will need to add the line `config.vm.box_version = "8.2.1"` after the line `config.vm.hostname = "ole"`.

#### Turn on the vagrant instance/Virtual Machine
```
vagrant up
```

You should be able to see the virtual machine on virtualbox once the script finishes
![virtualbox](http://people.sugarlabs.org/ignacio/virtualbox.png)
also you should be able to access to [http://127.0.0.1:5985/](http://127.0.0.1:5985/) to get a output like this one
(all in-line)

```
{
   "couchdb":"Welcome",
   "uuid":"e6da0e32f981846e88ff0d817e0d07bc",
   "version":"1.6.1",
   "vendor":{
      "name":"The Apache Software Foundation",
      "version":"1.6.1"
   }
}
```

### Generating the couchapp
Using `couchapp generate` command you will be able to generate the structure of a couchapp:
`couchapp generate appname`

In this case I will generate the app *"test2"*

![generate](http://people.sugarlabs.org/ignacio/generate.png)

The folder test2 ~~(appname)~~ will be created.

After you run it you will need to edit the file `.couchapprc`, you need to edit this file in order to add the server path:
This should be the content, where _db_ is the server path, in this case it is _http://127.0.0.1:5985_ and _myapp_ is the database where I want to insert the app

```
{ 
    "env" : {
        "myserver" : {
            "db" : "http://127.0.0.1:5985/myapp"
        } 
    }
}
```

Also you can edit some data, like _name_ and _description_  in the file `couchapp.json`

```
{
    "name": "Test app",
    "description": "Testing couchapps"
}
```

### Adding content to the app and pushing it
Now that you have generated the app, you will need to add the original app data ~~(the one you're porting)~~ to `_attachments` folder you may want to remove everything inside that folder before you add the new content, and then copy

```
rm -rf _attachments/*
cp -r /home/user/myoriginalapp/* _attachments/
```

Now that you have done all those steps you will need to upload the app to the server:<br>
`couchapp push myserver`<br>where `myserver` is the one you specified in the `.couchapprc` file.
![push](http://people.sugarlabs.org/ignacio/push.png)


The app is now available in the link specified, you should be able to see the database in http://127.0.0.1:5985/_utils/database.html?myapp
