# Summary

--Tensorflow默认会使用尽可能多的GPU，并且占用所使用的GPU。--因此如果有别的Tensorflow正在使用GPU，而自己的程序使用默认配置，那么是无法使用已经被使用的GPU的，也无法单独使用一块没有被使用的GPU。 
因此，我们可以在运行我们的tensorflow程序的时候，指定程序使用的特定GPU：

