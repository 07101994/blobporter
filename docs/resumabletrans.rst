Resumable Transfers
===================

BlobPorter supports resumable transfers. This feature is enabled when the -l option is set with the path where the transfer status file will be created.
In case of failure, if the same status file is specified, BlobPorter will skip files that were already transferred.

::

    blobporter -f "manyfiles/*" -c many -l mylog

For each file in the transfer two entries will be created in the status file.  One when file is queued (Started) and another when the file is successfully transferred (Completed).

The log entries are created with the following tab-delimited format:

::

    [Timestamp] [Filename] [Status (1:Started,2:Completed,3:Ignored)] [Size] [Transfer ID ]


The following output from a transfer status file shows that three files were included in the transfer:  **file10** ,  **file11**  and  **file15** .
However, only  **file10**  and  **file11**  were successfully transferred.  For  **file15**  the output indicates that it was queued but there's no second entry confirming that it was transferred successfully (status = 2). ::

    2018-03-05T03:31:13.034245807Z  file10  1       104857600       938520246_mylog
    2018-03-05T03:31:13.034390509Z  file11  1       104857600       938520246_mylog
    2018-03-05T03:31:13.034437109Z  file15  1       104857600       938520246_mylog
    2018-03-05T03:31:25.232572306Z  file10  2       104857600       938520246_mylog
    2018-03-05T03:31:25.591239355Z  file11  2       104857600       938520246_mylog

Consider the previous scenario and assume that the transfer was executed again.
In this case, the status file shows two new entries for  **file15**  in a new transfer (the transfer ID is different) which is an indication that this time the file was transferred successfully. ::

    2018-03-05T03:31:13.034245807Z  file10  1       104857600       938520246_mylog
    2018-03-05T03:31:13.034390509Z  file11  1       104857600       938520246_mylog
    2018-03-05T03:31:13.034437109Z  file15  1       104857600       938520246_mylog
    2018-03-05T03:31:25.232572306Z  file10  2       104857600       938520246_mylog
    2018-03-05T03:31:25.591239355Z  file11  2       104857600       938520246_mylog
    2018-03-05T03:54:33.660161772Z  file15  1       104857600       495675852_mylog
    2018-03-05T03:54:34.579295059Z  file15  2       104857600       495675852_mylog

Finally, since the process completed successfully, a summary is appended to the transfer status file. ::

    ----------------------------------------------------------
    Transfer Completed----------------------------------------
    Start Summary---------------------------------------------
    Last Transfer ID:495675852_mylog
    Date:Mon Mar  5 03:54:34 UTC 2018
    File:file15     Size:104857600  TID:495675852_mylog
    File:file10     Size:104857600  TID:938520246_mylog
    File:file11     Size:104857600  TID:938520246_mylog
    Transferred Files:3     Total Size:314572800
    End Summary-----------------------------------------------



 

