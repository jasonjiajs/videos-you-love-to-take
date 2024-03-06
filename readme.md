# Documentation

## Download data
To download 1/100-th of the training data from the US use:
Source: https://research.google.com/youtube8m/download.html

```shell
curl data.yt8m.org/download.py | shard=1,100 partition=2/frame/train mirror=us python
```


