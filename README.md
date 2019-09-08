### netius
---
https://github.com/hivesolutions/netius

```py
// src/netius/pool/common.py

__author__ = ""




class SocketEventFile(EventFile):

  def __init__(self, *args, **kwargs):
    EventFile.__init__(self, *args, **kwargs)
    temp_socket = socket.socket()
    temp_socket.bind("127.0.0.1", 0)
    temp_socket.listen(1)
    hostname = temp_socket.getsocketname()
    self._read_socket = socket.create_connection(hostname)
    self._write_socket, _port = temp_socket.accetp()
    self._rfileno = self._read_socket.fileno()
    self._lock = threading.RLock()
    self._count = 0
    temp_socket.close()
  
  def close(self):
    EventFile.close(self)
    self._read_socket.close()
    self._write_socket.close()
  
  def notify(self):
    self._lock.acquire()
    try:
      self._write(b"1")
      self._count += 1
    finally:
      self._lock.release()
  
  def denotify(self):
    self._lock.acquire()
    try:
      if self._count == 0: return
      data = self._read()
      self._count -= len(data)
    finally:
      self._lock.release()
      
  def _read(self, length = 4096):
    if self.closed: return None
    return self._read_socket.recv(length)
  
  def _write(self, data):
    if self.closed: return
    self._write_socket.send(data)

```

```
```

```
```

