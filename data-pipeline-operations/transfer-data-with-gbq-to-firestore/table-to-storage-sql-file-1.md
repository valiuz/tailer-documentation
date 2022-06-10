---
description: >-
  This is the description of the SQL file used for a Table to Storage data
  operation.
---

# File-to-firestore python file

To run a GBQ to Firestore data operation, you need a precise python script like this one, used for the example.

## :eye\_in\_speech\_bubble: Example of file-to-firestore.py :

```python
import argparse
import os
import fnmatch
import datetime
import pytz
import simplejson as json
from google.cloud import firestore


if __name__ == "__main__":

    parser = argparse.ArgumentParser(
        description=__doc__, 
        formatter_class=argparse.RawDescriptionHelpFormatter)

    parser.add_argument("source_gcs", help="GCS Bucket.", type=str)
    parser.add_argument("items", help="GCS Bucket.", type=str)

    args = parser.parse_args()

    source_directory = args.source_gcs.strip().strip("/")

    # Process items
    #
    files_to_process_infos = json.loads(args.items)

    print("GCS source       : {}".format(source_directory))
    print("Items to process : {}".format(files_to_process_infos))

    fs_client = firestore.Client(project="project_id")
    batch = firestore.WriteBatch(client=fs_client)

    for item_to_process in files_to_process_infos.keys():

        print("\nProcessing : {}".format(item_to_process))

        for root, dirs, files in os.walk(source_directory + "/" + files_to_process_infos[item_to_process]["sub_dir"]):

            for filename in fnmatch.filter(files, files_to_process_infos[item_to_process]["file_template"]):

                full_filename = source_directory + "/" + files_to_process_infos[item_to_process]["sub_dir"] + "/" + filename 

                with open(full_filename, "r", encoding="utf-8") as input_file:

                    print("Processing file {}".format(full_filename))

                    batch_index = 1
                    total_writes = 0
                    payload = {}

                    for line in input_file.readlines():

                        payload = json.loads(line)
                        payload["update_time"] = datetime.datetime.now(pytz.timezone("UTC"))

                        # build document path
                        #
                        doc_ref = None
                        for fs_path_index, fs_item_path in enumerate(payload["firestore_path"].split("|")):

                            # First pass to instantiate object
                            #
                            if fs_path_index == 0:
                                doc_ref = fs_client.collection(fs_item_path)
                                continue

                            # Add collection or document
                            #
                            if fs_path_index % 2 == 0:

                                # collection
                                #
                                doc_ref = doc_ref.collection(fs_item_path)

                            else:

                                # document
                                #
                                doc_ref = doc_ref.document(fs_item_path)

                        if batch_index >= 499:
                            total_writes += batch_index
                            batch.commit()
                            batch_index = 1
                            print(total_writes)
                        else:
                            batch_index += 1

                        del payload["firestore_path"]
                        batch.set(doc_ref, payload, merge=True)

                    # Final commit
                    #
                    batch.commit()

                    total_writes += batch_index
                    print(total_writes)
```

## :globe\_with\_meridians: Global parameters

**To use the python deployment automation on Firestore, the SQL must follow a specific pattern.**

| Variables                                                        | Descriptions                                                                                                        |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| <p><strong>project_id</strong></p><p>line 30</p><p>mandatory</p> | For your different use cases, it can be interesting to have the last calculation date of your dataset to Firestore. |

**Except for the project\_id variable, nothing needs to be changed. You can copy and paste the code in your python file for this data operation.**

