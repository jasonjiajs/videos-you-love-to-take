# Documentation

## Download data
Make data folders
```shell
mkdir -p data/yt8m/frame; cd data/yt8m/frame
```

Download 1/N-th of the training data from the US, e.g. N=1000 (note: full dataset is 1.5TB)
```shell
curl data.yt8m.org/download.py | shard=1,100 partition=2/frame/train mirror=us python
```
Source: https://research.google.com/youtube8m/download.html

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

