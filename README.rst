PureFTP Complete Upload Script
==============================

**Upload script creating a .complete file on complete file upload.**

Configuration
-------------

#. Enable PureFTP's upload script option:

    sudo echo "yes" > /etc/pure-ftpd/conf/CallUploadScript

#. Run PureFTP in standalone mode (not inetd mode). In /etc/default/pure-ftpd-common set::

    STANDALONE_OR_INETD=standalone

#. Set the user ID under which the script should be run. It is suggested that this be set to the ftpuser id::

    $ if ftpuser
    uid=1021(ftpuser) gid=1022(ftpgroup) groups=1022(ftpgroup)

In /etc/default/pure-ftpd-common set::
    UPLOADUID=1021
    UPLOADGID=1022

#. Tell pure-ftpd where the script to execute is located. Buildout creates an upload script for each instance under the buildout root folder's pureftp path. In /etc/default/pure-ftpd-common set::
    
    UPLOADSCRIPT=/home/you/buildout-path/pureftp/upload_script_production_main.sh

#. Restart PureFTP::

    $ sudo /etc/init.d/pure-ftpd restart
    Restarting ftp server: Running: /usr/sbin/pure-ftpd -l pam -l puredb:/etc/pure-ftpd/pureftpd.pdb -E -O clf:/var/log/pure-ftpd/transfer.log -u 1000 -o -8 UTF-8 -B
    Restarting ftp upload handler: pure-uploadscript.

#. Confirm daemon operation::
    
    $ ps aux | grep pure-uploadscript 
    ftpuser 18671 0.0 0.0 11912 672 ? Ss 19:40 0:00 /usr/sbin/pure-uploadscript -r <upload script path here> -B -u 1021 -g 1022

