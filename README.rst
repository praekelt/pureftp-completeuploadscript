PureFTP Complete Upload Script
==============================

**Upload script creating a <uploaded_filename>.complete file on complete file upload.**

Configuration
-------------

#. Enable PureFTP's upload script option::

    $ sudo echo "yes" > /etc/pure-ftpd/conf/CallUploadScript

#. Run PureFTP in standalone mode (not inetd mode). In /etc/default/pure-ftpd-common set:

    STANDALONE_OR_INETD=standalone

#. Set the user and group ID under which the upload script should be run (it's suggested that this be set to the ``ftpuser`` and ``ftpgroup`` ids. In /etc/default/pure-ftpd-common set:

    UPLOADUID=1021
    
    UPLOADGID=1022

#. (You can retrieve ftpuser and ftpgroup ids by running)::

    $ if ftpuser
    uid=1021(ftpuser) gid=1022(ftpgroup) groups=1022(ftpgroup)

#. Set the upload script path. In /etc/default/pure-ftpd-common set:
    
    UPLOADSCRIPT=<path/to/complete_upload_script.sh>

#. Restart PureFTP::

    $ sudo /etc/init.d/pure-ftpd restart
    Restarting ftp server: Running: /usr/sbin/pure-ftpd -l pam -l puredb:/etc/pure-ftpd/pureftpd.pdb -E -O clf:/var/log/pure-ftpd/transfer.log -u 1000 -o -8 UTF-8 -B
    Restarting ftp upload handler: pure-uploadscript.

#. Confirm daemon operation::
    
    $ ps aux | grep pure-uploadscript 
    ftpuser 18671 0.0 0.0 11912 672 ? Ss 19:40 0:00 /usr/sbin/pure-uploadscript -r <upload script path here> -B -u 1021 -g 1022

