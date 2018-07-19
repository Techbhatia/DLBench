# Caltech 101

[The Caltech 101 dataset](http://www.vision.caltech.edu/Image_Datasets/Caltech101/)


## Caffe:

### Convert the raw data into the LMDB format:

1. Change directory to datasets:
   ````
   cd tutorials/datasets/
   ````
2. Download [CIFAR-100 python version](https://www.cs.toronto.edu/~kriz/cifar-100-python.tar.gz) 
   ````
   wget https://www.cs.toronto.edu/~kriz/cifar-100-python.tar.gz
   ```` 
3. Extract files
   ````
   tar -xf cifar-100-python.tar.gz && rm -f cifar-100-python.tar.gz
   ````
4. Generate LMDB files (Install missing libraries for Python)
   ````
   python convert_cifar100_lmdb.py   
   ````
### Use CIFAR-100 in LMDB format:
Add the following data layer definition to the network *prototxt* file.

````
layer {
  name: "cifar100"
  type: "Data"
  top: "data"
  top: "label"
  include {
    phase: TRAIN
  }
  transform_param {
    mean_file: "../mean.binaryproto"
  }
  data_param {
    source: "../cifar100_train_lmdb"
    batch_size: 128
    backend: LMDB
  }
}
layer {
  name: "cifar100"
  type: "Data"
  top: "data"
  top: "label"
  include {
    phase: TEST
  }
  transform_param {
    mean_file: "../mean.binaryproto"
  }
  data_param {
    source: "../cifar100_test_lmdb"
    batch_size: 100
    backend: LMDB
  }
}
````

## General tools for Python

Modify the input file *cifar10_input.py* described in TensorFlow [@TensorFlow_Convolutional_Neural_Networks] to support the CIFAR-100 dataset.

### Official Python function:
    def unpickle(file):
        import cPickle
        with open(file, 'rb') as fo:
            dict = cPickle.load(fo)
    return dict



[@Github: uncommon-datasets-caffe]: https://github.com/junyuseu/uncommon-datasets-caffe
[@TensorFlow_Convolutional_Neural_Networks]:https://www.tensorflow.org/tutorials/deep_cnn
