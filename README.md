# UTS Pengolahan Citra (Membuat-GUI-pada-MATLAB)

~~~
Nama        : Isnaini Rizkyana
Kelas       : TI.19.C1
NIM         : 311910254
Mata Kuliah : Pengolahan Citra
Dosen       : Muhammad Najamuddin Dwi Miharja, M.Kom
~~~

# Source Code

~~~
function varargout = UTS(varargin)
% UTS MATLAB code for UTS.fig
%      UTS, by itself, creates a new UTS or raises the existing
%      singleton*.
%
%      H = UTS returns the handle to a new UTS or the handle to
%      the existing singleton*.
%
%      UTS('CALLBACK',hObject,eventData,handles,...) calls the local
%      function named CALLBACK in UTS.M with the given input arguments.
%
%      UTS('Property','Value',...) creates a new UTS or raises the
%      existing singleton*.  Starting from the left, property value pairs are
%      applied to the GUI before UTS_OpeningFcn gets called.  An
%      unrecognized property name or invalid value makes property application
%      stop.  All inputs are passed to UTS_OpeningFcn via varargin.
%
%      *See GUI Options on GUIDE's Tools menu.  Choose "GUI allows only one
%      instance to run (singleton)".
%
% See also: GUIDE, GUIDATA, GUIHANDLES

% Edit the above text to modify the response to help UTS

% Last Modified by GUIDE v2.5 05-May-2021 09:33:10

% Begin initialization code - DO NOT EDIT
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @UTS_OpeningFcn, ...
                   'gui_OutputFcn',  @UTS_OutputFcn, ...
                   'gui_LayoutFcn',  [] , ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
    gui_State.gui_Callback = str2func(varargin{1});
end

if nargout
    [varargout{1:nargout}] = gui_mainfcn(gui_State, varargin{:});
else
    gui_mainfcn(gui_State, varargin{:});
end
% End initialization code - DO NOT EDIT


% --- Executes just before UTS is made visible.
function UTS_OpeningFcn(hObject, eventdata, handles, varargin)
% This function has no output args, see OutputFcn.
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
% varargin   command line arguments to UTS (see VARARGIN)

% Choose default command line output for UTS
handles.output = hObject;

% Update handles structure
guidata(hObject, handles);

% UIWAIT makes UTS wait for user response (see UIRESUME)
% uiwait(handles.figure1);


% --- Outputs from this function are returned to the command line.
function varargout = UTS_OutputFcn(hObject, eventdata, handles) 
% varargout  cell array for returning output args (see VARARGOUT);
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Get default command line output from handles structure
varargout{1} = handles.output;


% --- Executes on button press in pushbutton1.
function pushbutton1_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
[filename,pathname] = uigetfile({'*.*'});
 
if ~isequal(filename,0)
    Info = imfinfo(fullfile(pathname,filename));
    if Info.BitDepth == 24
        Img = imread(fullfile(pathname,filename));
        axes(handles.axes1)
        cla('reset')
        imshow(Img)
        title('RGB Image')
    else
        msgbox('Citra masukan harus citra RGB');
        return
    end
else
    return
end
 
handles.Img = Img;
guidata(hObject,handles);
 
set(handles.radiobutton1,'Enable','on')
set(handles.radiobutton2,'Enable','on')
set(handles.radiobutton3,'Enable','on')
set(handles.radiobutton4,'Enable','on')
set(handles.radiobutton5,'Enable','on')
set(handles.radiobutton6,'Enable','on')
set(handles.slider1,'Enable','on')
set(handles.slider2,'Enable','on')
set(handles.edit1,'Enable','on')
 
axes(handles.axes2)
cla('reset')
set(gca,'XTick',[])
set(gca,'YTick',[])

% --- Executes on slider movement.
function slider1_Callback(hObject, eventdata, handles)
% hObject    handle to slider1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider
set(handles.radiobutton1,'Value',0)
set(handles.radiobutton2,'Value',0)
set(handles.radiobutton3,'Value',0)
set(handles.radiobutton4,'Value',0)
set(handles.radiobutton5,'Value',0)
set(handles.radiobutton6,'Value',0)
set(handles.popupmenu1,'Enable','off')
 
Img = handles.Img;
Gray = rgb2gray(Img);
 
thresh = get(hObject,'Value');
bw = im2bw(Gray,thresh);
 
axes(handles.axes1)
cla('reset')
imshow(Gray)
title('Grayscale Image')
axes(handles.axes2)
cla('reset')
imshow(bw)
title('Binary Image')
set(handles.edit1,'String',num2str(thresh))

% --- Executes during object creation, after setting all properties.
function slider1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to slider1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end


% --- Executes on slider movement.
function slider2_Callback(hObject, eventdata, handles)
% hObject    handle to slider2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'Value') returns position of slider
%        get(hObject,'Min') and get(hObject,'Max') to determine range of slider
nilai_slider = get(handles.slider2,'value');
citra = double(handles.data1);
 
bitdepth = handles.bitdepth;
[m,n,~] = size(handles.data1);
 
if bitdepth == 8
    for i = 1:m
        for j = 1:n
            citra(i,j) = citra(i,j)+nilai_slider;
            if citra(i,j) > 255
                citra(i,j) = 255;
            end;
            if citra(i,j) < 0
                citra(i,j) = 0;
            end;
        end;
    end;
elseif bitdepth == 24
    for x = 1:3
        for i = 1:m
            for j = 1:n
                citra(i,j,x) = citra(i,j,x)+nilai_slider;
                if citra(i,j,x) > 255
                    citra(i,j,x) = 255;
                end;
                if citra(i,j,x) < 0
                    citra(i,j,x) = 0;
                end;
            end;
        end;
    end
else
    msgbox('Citra masukan harus grayscale atau RGB');
end

axes(handles.axes1)
title('Citra Asli')
axes(handles.axes2);
gambar = uint8(citra);
imshow(gambar)
title('Citra Hasil Pergeseran');

% --- Executes during object creation, after setting all properties.
function slider2_CreateFcn(hObject, eventdata, handles)
% hObject    handle to slider2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: slider controls usually have a light gray background.
if isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor',[.9 .9 .9]);
end



function edit1_Callback(hObject, eventdata, handles)
% hObject    handle to edit1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'String') returns contents of edit1 as text
%        str2double(get(hObject,'String')) returns contents of edit1 as a double


% --- Executes during object creation, after setting all properties.
function edit1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to edit1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: edit controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end


% --- Executes on selection change in popupmenu1.
function popupmenu1_Callback(hObject, eventdata, handles)
% hObject    handle to popupmenu1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: contents = cellstr(get(hObject,'String')) returns popupmenu1 contents as cell array
%        contents{get(hObject,'Value')} returns selected item from popupmenu1
Value = get(hObject,'Value');
 
Gray = handles.Gray;
axes(handles.axes1)
cla('reset')
imshow(Gray)
title('Grayscale Image')
switch Value
    case 1
        colormap gray
    case 2
        colormap parula
    case 3
        colormap jet
    case 4
        colormap hsv
    case 5
        colormap hot
    case 6
        colormap cool
    case 7
        colormap spring
    case 8
        colormap summer
    case 9
        colormap autumn
    case 10
        colormap winter
    case 11
        colormap bone
    case 12
        colormap copper
    case 13
        colormap pink
end

% --- Executes during object creation, after setting all properties.
function popupmenu1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to popupmenu1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: popupmenu controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end


% --- Executes on button press in radiobutton1.
function radiobutton1_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hint: get(hObject,'Value') returns toggle state of radiobutton1
set(handles.radiobutton1,'Value',1)
set(handles.radiobutton2,'Value',0)
set(handles.radiobutton3,'Value',0)
set(handles.radiobutton4,'Value',0)
set(handles.radiobutton5,'Value',0)
set(handles.radiobutton6,'Value',0)
set(handles.slider1,'Value',0)
set(handles.slider2,'Value',0)
set(handles.edit1,'String',0)
set(handles.popupmenu1,'Enable','off')
 
Img = handles.Img;
R = Img(:,:,1);
G = Img(:,:,2);
B = Img(:,:,3);
Red_Channel = cat(3,R,G*0,B*0);
axes(handles.axes1)
cla('reset')
imshow(Red_Channel)
title('Red Channel')
axes(handles.axes2)
cla('reset')
h = histogram(R(:));
h.FaceColor = [1 0 0];
xlim([0 255])
grid on
title('Histogram')


% --- Executes on button press in radiobutton2.
function radiobutton2_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hint: get(hObject,'Value') returns toggle state of radiobutton2
set(handles.radiobutton1,'Value',0)
set(handles.radiobutton2,'Value',1)
set(handles.radiobutton3,'Value',0)
set(handles.radiobutton4,'Value',0)
set(handles.radiobutton5,'Value',0)
set(handles.radiobutton6,'Value',0)
set(handles.slider1,'Value',0)
set(handles.slider2,'Value',0)
set(handles.edit1,'String',0)
set(handles.popupmenu1,'Enable','off')
 
Img = handles.Img;
R = Img(:,:,1);
G = Img(:,:,2);
B = Img(:,:,3);
Green_Channel = cat(3,R*0,G,B*0);
axes(handles.axes1)
cla('reset')
imshow(Green_Channel)
title('Green Channel')
axes(handles.axes2)
cla('reset')
h = histogram(G(:));
h.FaceColor = [0 1 0];
xlim([0 255])
grid on
title('Histogram')


% --- Executes on button press in radiobutton3.
function radiobutton3_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton3 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hint: get(hObject,'Value') returns toggle state of radiobutton3
set(handles.radiobutton1,'Value',0)
set(handles.radiobutton2,'Value',0)
set(handles.radiobutton3,'Value',1)
set(handles.radiobutton4,'Value',0)
set(handles.radiobutton5,'Value',0)
set(handles.radiobutton6,'Value',0)
set(handles.slider1,'Value',0)
set(handles.slider2,'Value',0)
set(handles.edit1,'String',0)
set(handles.popupmenu1,'Enable','off')
 
Img = handles.Img;
R = Img(:,:,1);
G = Img(:,:,2);
B = Img(:,:,3);
Blue_Channel = cat(3,R*0,G*0,B);
axes(handles.axes1)
cla('reset')
imshow(Blue_Channel)
title('Blue Channel')
axes(handles.axes2)
cla('reset')
h = histogram(B(:));
h.FaceColor = [0 0 1];
xlim([0 255])
grid on
title('Histogram')


% --- Executes on button press in radiobutton4.
function radiobutton4_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton4 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hint: get(hObject,'Value') returns toggle state of radiobutton4
set(handles.radiobutton1,'Value',0)
set(handles.radiobutton2,'Value',0)
set(handles.radiobutton3,'Value',0)
set(handles.radiobutton4,'Value',1)
set(handles.radiobutton5,'Value',0)
set(handles.radiobutton6,'Value',0)
set(handles.slider1,'Value',0)
set(handles.slider2, 'Value',0)
set(handles.edit1,'String',0)
set(handles.popupmenu1,'Enable','on')
 
Img = handles.Img;
Gray = rgb2gray(Img);
 
Value = get(handles.popupmenu1,'Value');
 
axes(handles.axes1)
cla('reset')
imshow(Gray)
title('Grayscale Image')
switch Value
    case 1
        colormap gray
    case 2
        colormap parula
    case 3
        colormap jet
    case 4
        colormap hsv
    case 5
        colormap hot
    case 6
        colormap cool
    case 7
        colormap spring
    case 8
        colormap summer
    case 9
        colormap autumn
    case 10
        colormap winter
    case 11
        colormap bone
    case 12
        colormap copper
    case 13
        colormap pink
end
 
axes(handles.axes2)
cla('reset')
h = histogram(Gray(:));
h.FaceColor = [0.5 0.5 0.5];
xlim([0 255])
grid on
title('Histogram')
 
handles.Gray = Gray;
guidata(hObject,handles)


% --- Executes on button press in radiobutton5.
function radiobutton5_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton5 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hint: get(hObject,'Value') returns toggle state of radiobutton5
set(handles.radiobutton1,'Value',0)
set(handles.radiobutton2,'Value',0)
set(handles.radiobutton3,'Value',0)
set(handles.radiobutton4,'Value',0)
set(handles.radiobutton5,'Value',1)
set(handles.radiobutton6,'Value',0)
set(handles.slider1,'Value',0)
set(handles.slider2,'Value',0)
set(handles.edit1,'String',0)
set(handles.popupmenu1,'Enable','off')
 
Img = handles.Img;
Gray = rgb2gray(Img);
thresh = graythresh(Gray);
bw = im2bw(Gray,thresh);
 
axes(handles.axes1)
cla('reset')
imshow(Gray)
title('Grayscale Image')
axes(handles.axes2)
cla('reset')
imshow(bw)
title('Binary Image')
set(handles.edit1,'String',thresh)


% --- Executes on button press in radiobutton6.
function radiobutton6_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton6 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hint: get(hObject,'Value') returns toggle state of radiobutton6
set(handles.radiobutton1,'Value',0)
set(handles.radiobutton2,'Value',0)
set(handles.radiobutton3,'Value',0)
set(handles.radiobutton4,'Value',0)
set(handles.radiobutton5,'Value',0)
set(handles.radiobutton6,'Value',1)
set(handles.popupmenu1,'Enable','off')
 
Img = handles.Img;
axes(handles.axes1)
imshow(Img)
title('RGB Image')
R = Img(:,:,1);
G = Img(:,:,2);
B = Img(:,:,3);
axes(handles.axes2)
cla('reset')
h = histogram(R(:));
h.FaceColor = [1 0 0];
xlim([0 255])
grid on
hold on
h = histogram(G(:));
h.FaceColor = [0 1 0];
xlim([0 255])
h = histogram(B(:));
h.FaceColor = [0 0 1];
xlim([0 255])
hold off
title('Histogram')


% --- Executes on button press in pushbutton2.
function pushbutton2_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
set(handles.slider2,'value',0);
[name_file1,name_path1] = uigetfile( ...
    {'*.bmp;*.jpg;*.tif','Files of type (*.bmp,*.jpg,*.tif)';
    '*.bmp','File Bitmap (*.bmp)';...
    '*.jpg','File jpeg (*.jpg)';
    '*.tif','File Tif (*.tif)';
    '*.*','All Files (*.*)'},...
    'Open Image');
 
if ~isequal(name_file1,0)
    handles.data1 = imread(fullfile(name_path1,name_file1));
    axes(handles.axes1);
    imshow(handles.data1);
    axes(handles.axes2);
    imshow(handles.data1);
    info = imfinfo(fullfile(name_path1,name_file1));
    bitdepth = info.BitDepth;
    handles.bitdepth = bitdepth;
    guidata(hObject,handles);
else
    return;
end


% --- Executes on mouse press over figure background.
function figure1_ButtonDownFcn(hObject, eventdata, handles)
% hObject    handle to figure1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

~~~

