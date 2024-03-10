# What videos do you love to take? - Personal video classification with deep learning
Final Project, MIT 15.773 - Hands-on Deep Learning <br>
Team Members: [Jason Jia](https://www.linkedin.com/in/jasonjiajs/), [Maxime Wolf](https://www.linkedin.com/in/maxime-wolf/), [Maria Besedovskaya](https://www.linkedin.com/in/mariabesedovskaya/), [Minnie Arunaramwong](https://www.linkedin.com/in/minnie-arunaramwong/)


## Download data
Here we just work with frame-level data and not video-level data.

1. Make data folders
```shell
mkdir -p data/yt8m/frame; cd data/yt8m/frame
```

2. Download 1/N-th of the data from the US (we use N=300). This gives 29 training files. (note: full frame-level dataset is 1.5TB). The dataset can be found at https://research.google.com/youtube8m/download.html.

```shell
curl data.yt8m.org/download.py | shard=1,300 partition=2/frame/train mirror=us python
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

