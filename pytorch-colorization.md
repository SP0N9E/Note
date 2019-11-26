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
torch.cat将这些切片得到的东西进行组合，并在维度1上进行拼接。注意，维度0直观上看是二维数组中的列，维度1直观上看是二维数组中的行，因此这里是将他们进行横向拼接。
函数rgb2xyz()返回torch.cat拼接完成的张量tensor

##### util.py        函数xyz2lab()
思路同上rgb2xyz
具体参见[xyz2rgb](https://blog.csdn.net/tian_110/article/details/45575869)
返回数据格式也相同

##### util.py        函数rgb2lab()
```python
	lab = xyz2lab(rgb2xyz(rgb))
    l_rs = (lab[:,[0],:,:]-opt.l_cent)/opt.l_norm
    ab_rs = lab[:,1:,:,:]/opt.ab_norm
    out = torch.cat((l_rs,ab_rs),dim=1)
    # if(torch.sum(torch.isnan(out))>0):
        # print('rgb2lab')
        # embed()
    return out
```
分别取出L通道、AB通道后进行归一化处理，再通过torch.cat在维度1上进行连接，完成后返回连接后的tensor


##### torch.max()
```python
	torch.max(data, dim=?)
```
dim = 0时，选取的是某一列中最大的元素。返回该最大元素值以及在此维度中的下标
dim = 1时，选取的是某一行中最大的元素。返回该最大元素值以及在此维度中的下标


##### utils.py          函数get_colorization_data()
```python
	torch.max(data['B'],dim=3)[0]  # 在data[‘B’]中选取最里面一层（其实就是每一行）的最大值,返回的是一个三维数组
    torch.max(torch.max(data['B'],dim=3)[0],dim=2)[0]  # 再次选取返回三维数组中最里面那一层每一行的最大值，返回的是二维数组 简而言之这里得到的就是二维矩阵（图像中）最大的值
    torch.max(torch.max(data['B'],dim=3)[0],dim=2)[0]-torch.min(torch.min(data['B'],dim=3)[0],dim=2)[0]) # 二位图像中最大最小值的差
    
```













