# -Dicom-
在读取Dicom文件和将二维数组显示成图像方面的经验

在读取Dicom文件的时候可以使用Evil-dicom这个开元库。

将二维数组或矩阵显示成图像可以借鉴以下C++代码。
public Bitmap GetBitMap(Matrix _matrix,bool isMap )
       {
           if (_matrix == null)
           {
               throw new ImageMapException("图片数据为空！！");
           }
           Bitmap bmp = new Bitmap(_matrix.ColCount, _matrix.RowCount);
           for (int i = 0; i < bmp.Width; i++)
           {
               for (int j = 0; j < bmp.Height; j++)
               {
                   Color tmpColor;
                   if (!isMap)
                   {
                       tmpColor = Color.FromArgb(_matrix[i, j]);
                   }
                   else
                   {
                       if (_matrix[i, j] == 0)
                       {
                           tmpColor = Color.FromArgb(IM_WHITE);
                       }
                       else
                       {
                           tmpColor = Color.FromArgb(IM_BLACK);
                       }
                   }
                   bmp.SetPixel(i, j, tmpColor);
               }
           }
           return bmp;
       }
       
注意：通过矩阵里面的数值获得的Color也许是透明的，此时可以将每个二维数组的值加上0xff000000，使获得的颜色不透明，参考以下C#代码
for(int i = 0; i < floatar.GetLength(0); i++)
            {
                for(int j=0;j< floatar.GetLength(2); j++)
                {
                    unchecked
                    {
                        fnewar2[i, j] = (int)floatar[i,60, j] + (int)4278190080;//4278190080是oxff000000的十进制值
                    }
                    
                    //fnewar[i, j] = 4278190080;
                }
            }
