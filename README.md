
## modified ultralytics src code
### vim /usr/local/lib/python3.10/dist-packages/ultralytics/nn/tasks.py 
    BaseModel._predict_once
    in end add:
```
        if isinstance(x, torch.Tensor):
            # import pdb
            # pdb.set_trace()
            x = x.transpose(2, 1)
        return x
```
### export onnx:
```
from ultralytics import YOLO
model = YOLO("${path}/yolo11n.pt")
success = model.export(format="onnx", simplify=True)  # export the model to onnx format
```
### build engine:
```
trtexec --onnx=${path}/yolo11n.onnx --saveEngine=yolo11n.engine
```
