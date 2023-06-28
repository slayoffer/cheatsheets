To download an Azure Blob file with `wget`, you can use a Shared Access Signature (SAS) URL that grants read access to the blob. Here are the steps to download an Azure Blob file with `wget`:

1. Obtain a SAS URL for the blob you want to download. You can create a SAS URL in the Azure portal or using an Azure Storage client library.

2. Open a terminal window on your Linux system.

3. Run the `wget` command with the SAS URL as the argument:

   ```
   wget "<SAS URL>"
   ```

4. The file will be downloaded to the current directory.

   ```
   HTTP request sent, awaiting response... 200 OK
   Length: 9702 (9.5K) [text/plain]
   Saving to: ‘<filename>’
   ```

In the above example, `wget` downloads the file specified by the SAS URL and saves it to the current directory with the original file name.

Note that you need to enclose the SAS URL in quotes because it contains special characters that may be interpreted by the shell. You also need to make sure that the SAS URL grants read access to the blob. If the SAS URL expires, you will need to obtain a new SAS URL and download the file again.