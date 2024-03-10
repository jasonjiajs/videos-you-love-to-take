# What videos do you love to take?


## Download data
Here we just work with frame-level data and not video-level data.

Make data folders
```shell
mkdir -p data/yt8m/frame; cd data/yt8m/frame
```

Download 1/N-th of the data from the US, e.g. N=1000. This gives 5 training files, 5 validation files and 4 test files (note: full frame-level dataset is 1.5TB).

Source: https://research.google.com/youtube8m/download.html

```shell
curl data.yt8m.org/download.py | shard=1,1000 partition=2/frame/train mirror=us python
curl data.yt8m.org/download.py | shard=1,1000 partition=2/frame/validate mirror=us python
curl data.yt8m.org/download.py | shard=1,1000 partition=2/frame/test mirror=us python
```

You should see output similar to the following:
```
Starting fresh download in this directory. Please make sure you have >2TB of free disk space!
>> Downloading http://data.yt8m.org/2/download_plans/frame_validate.json 101.1%Succesfully downloaded 2_frame_validate_download_plan.json 226807 bytes.
Files remaining 5
```

## Data format
- Frame-level features:
```
context: {
  feature: {
    key  : "id"
    value: {
      bytes_list: {
        value: (Video id)
      }
    }
  }
  feature: {
    key  : "labels"
      value: {
        int64_list: {
          value: [1, 522, 11, 172]  # label list
        }
      }
    }
}

feature_lists: {
  feature_list: {
    key  : "rgb"
    value: {
      feature: {
        bytes_list: {
          value: [1024 8bit quantized features]
        }
      }
      feature: {
        bytes_list: {
          value: [1024 8bit quantized features]
        }
      }
      ... # Repeated for every second, up to 300
  }
  feature_list: {
    key  : "audio"
    value: {
      feature: {
        bytes_list: {
          value: [128 8bit quantized features]
        }
      }
      feature: {
        bytes_list: {
          value: [128 8bit quantized features]
        }
      }
    }
    ... # Repeated for every second, up to 300
  }

}

```
Installing opencv:
```
pip3 install opencv-python
```
Check that running this command prints True (where you have a video `./sample-data/sample-video-1.mp4`):
```
python -c 'import cv2; print(cv2.VideoCapture().open("./sample-data/sample-video-1.mp4"))'
```

Then, you can create the tfrecord by calling the binary:
```
python code/extract_tfrecords_main.py --input_videos_csv /sample-data/sample-data.csv \
    --output_tfrecords_file /sample-data/sample-data.tfrecord
```
