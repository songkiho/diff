# diff

1. 정상적인 df 파일 생성(기준값은 변동이 없어야한다)

```bash
# cat 00_as_is.sh
#!/bin/bash
cat /proc/mounts | grep xfs | awk '{print $2}' > /tmp/as_is.txt
```

1. DR전환 후 df 파일

```bash
# cat 01_to_be.sh
#!/bin/bash
cat /proc/mounts | grep xfs | awk '{print $2}' > /tmp/to_be.txt
```

1. as-is와 to-be 비교

```bash
# cat 03_diff.sh
#!/bin/sh
diff /tmp/as_is.txt /tmp/to_be.txt > /dev/null
if [ $? == 0 ]
  then
    echo "Normal"
  else
    diff as_is.txt to_be.txt  | grep -i \> | awk '{print $NF}' > /tmp/result.txt
    echo "Abnormal"
fi
```

1. 비정상적일 시 Abnormal 이 발생 자세한 결과는 result.txt 값을 확인

```bash
# cat /tmp/result.txt
/test1   <====== 기준값과 비교했을때 DR전환후 test1 마운트 포인트가 있음
```