### Pytorch colorization
##### util.py        函数rgb2xyz()
	rgb2xyz(rgb)	
    rgb = (((rgb+.055)/1.055)**2.4)*mask + rgb/12.92*(1-mask)
逆转换gamma矫正过程
具体参见[rgb2xyz](https://blog.csdn.net/scarecrow_wiscom/article/details/9795715)
``` python
	x = .412453*rgb[:,0,:,:]+.357580*rgb[:,1,:,:]+.180423*rgb[:,2,:,:]
    y = .212671*rgb[:,0,:,:]+.715160*rgb[:,1,:,:]+.072169*rgb[:,2,:,:]
    z = .019334*rgb[:,0,:,:]+.119193*rgb[:,1,:,:]+.950227*rgb[:,2,:,:]
```
rgb中第二个参数应该是通道
```python
	out = torch.cat((x[:,None,:,:],y[:,None,:,:],z[:,None,:,:]),dim=1)
```
None代表当前维度不进行切片(slice)，作为一个整体进行组合。直观上看会少一对**"[]"**
torch.cat将这些切片得到的东西进行组合，并在维度1上进行拼接。注意，维度0直观上看是二维数组中的行，维度1直观上看是二维数组中的列，因此这里是将他们进行横向拼接。
函数rgb2xyz()返回torch.cat拼接完成的张量tensor

##### util.py        函数xyz2lab()
思路同上rgb2xyz
具体参见[xyz2rgb](https://blog.csdn.net/tian_110/article/details/45575869)
返回数据格式也相同


















