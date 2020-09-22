<div align="center">

## A simple TakeScreenShot\(\) Function


</div>

### Description

The purpose of this function is to simply take a screenshot of the entire screen and save it as a bitmap file.
 
### More Info
 
filename - a string required to save the bitmap file to

Simply call the function; here is an example of how to use it:

#include &lt;iostream.h&gt;

#include &lt;windows.h&gt;

#include &lt;stdio.h&gt;

int main()

{

TakeScreenShot("c:\\Screenshot.bmp");

return 0;

}


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Adam Boulfoul](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/adam-boulfoul.md)
**Level**          |Beginner
**User Rating**    |4.3 (17 globes from 4 users)
**Compatibility**  |C\+\+ \(general\), Microsoft Visual C\+\+, Borland C\+\+
**Category**       |[Graphics/ Sound](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/graphics-sound__3-15.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/adam-boulfoul-a-simple-takescreenshot-function__3-10754/archive/master.zip)

### API Declarations

```
#include &lt;windows.h&gt;
#include &lt;stdio.h&gt;
```


### Source Code

```
void TakeScreenShot(char* filename)
{
	keybd_event(VK_SNAPSHOT, 0x45, KEYEVENTF_EXTENDEDKEY, 0);
	keybd_event(VK_SNAPSHOT, 0x45, KEYEVENTF_EXTENDEDKEY | KEYEVENTF_KEYUP, 0);
	HBITMAP h;
	OpenClipboard(NULL);
	h = (HBITMAP)GetClipboardData(CF_BITMAP);
	CloseClipboard();
	HDC hdc=NULL;
 FILE*  fp=NULL;
 LPVOID  pBuf=NULL;
 BITMAPINFO bmpInfo;
 BITMAPFILEHEADER bmpFileHeader;
 do
	{
		hdc=GetDC(NULL);
  ZeroMemory(&bmpInfo,sizeof(BITMAPINFO));
  bmpInfo.bmiHeader.biSize=sizeof(BITMAPINFOHEADER);
  GetDIBits(hdc,h,0,0,NULL,&bmpInfo,DIB_RGB_COLORS);
  if(bmpInfo.bmiHeader.biSizeImage<=0)
			bmpInfo.bmiHeader.biSizeImage=bmpInfo.bmiHeader.biWidth*abs(bmpInfo.bmiHeader.biHeight)*(bmpInfo.bmiHeader.biBitCount+7)/8;
  if((pBuf = malloc(bmpInfo.bmiHeader.biSizeImage))==NULL)
  {
   MessageBox( NULL, "Unable to Allocate Bitmap Memory", "Error", MB_OK|MB_ICONERROR);
	  break;
		}
  bmpInfo.bmiHeader.biCompression=BI_RGB;
  GetDIBits(hdc,h,0,bmpInfo.bmiHeader.biHeight,pBuf, &bmpInfo, DIB_RGB_COLORS);
  if((fp = fopen(filename,"wb"))==NULL)
  {
	  MessageBox( NULL, "Unable to Create Bitmap File", "Error", MB_OK|MB_ICONERROR);
   break;
  }
  bmpFileHeader.bfReserved1=0;
  bmpFileHeader.bfReserved2=0;
  bmpFileHeader.bfSize=sizeof(BITMAPFILEHEADER)+sizeof(BITMAPINFOHEADER)+bmpInfo.bmiHeader.biSizeImage;
  bmpFileHeader.bfType='MB';
  bmpFileHeader.bfOffBits=sizeof(BITMAPFILEHEADER)+sizeof(BITMAPINFOHEADER);
  fwrite(&bmpFileHeader,sizeof(BITMAPFILEHEADER),1,fp);
  fwrite(&bmpInfo.bmiHeader,sizeof(BITMAPINFOHEADER),1,fp);
  fwrite(pBuf,bmpInfo.bmiHeader.biSizeImage,1,fp);
	}
	while(false);
		if(hdc)  ReleaseDC(NULL,hdc);
  if(pBuf) free(pBuf);
  if(fp)  fclose(fp);
}
```

