# LevelDB Study  
*2022년 하계 방학 중 진행한 스터디*  
*Google에서 만든 Key-value store인 LevelDB를 소스코드 수준에서 분석하고,*    
*여러 가지 option들을 수정하며 성능 분석 및 튜닝을 진행하고 관련 문서들을 작성함*  

## 스터디 기간  
2022.07.05 ~ 2022.09.11  

## 스터디 내용  

*DKU System Software Lab의 홈페이지에서 스터디 내용들을 [전자책](https://sslab.dankook.ac.kr/leveldb-wiki/)으로도 편하게 읽어볼 수 있음.*  

### Analysis (at source code level)  
LevelDB에서 storage(disk)단에서 key-value pair들을 저장하는데 쓰이는 자료구조인 `SSTable`에 대해 소스 코드 수준에서 분석함.  
- [SSTable format](https://github.com/DKU-StarLab/leveldb-wiki/blob/main/analysis/sstable/sstable.md)
- [SSTable read](https://github.com/DKU-StarLab/leveldb-wiki/blob/main/analysis/sstable/sstable-read.md)
- [SSTable write](https://github.com/DKU-StarLab/leveldb-wiki/blob/main/analysis/sstable/sstable-write.md)  
- SSTable read 관련 분석 녹화본  
[![Video Label](http://img.youtube.com/vi/_D_YSKXDeRE/0.jpg)](https://www.youtube.com/watch?v=_D_YSKXDeRE)

### Benchmark  
여러 가지 option들을 주면서 각각의 상황에서 성능을 측정하고 그렇게 나온 이유에 대해 논의함.  
- [LevelDB benchmark 성능 분석 과제](https://github.com/DKU-StarLab/leveldb-study/tree/main/homework)  

이 외에도 Bloom Filter를 사용했을 때와 안 했을 때의 읽기 성능 차이 등을 측정함.  

`SSTable`과 관련하여, Bloom Filter를 쓰지 않으면 Filter Block을 쓰지 않으므로 쓰기 성능이 높아질 것이라 예측하고 이에 관한 성능 측정 및 분석도 진행함.
- [SSTable과 Bloom Filter 관련 쓰기 성능 분석](https://github.com/DKU-StarLab/leveldb-wiki/blob/main/benchmarks/sstable.md)  
  - [관련 발표자료](https://github.com/DKU-StarLab/leveldb-study/blob/main/benchmarks/2022.7.26_SSTable_week4.pdf)

### Tuning  
여러 option값들을 최적의 값으로 튜닝하고, 각 workload 수행의 평균 throughput이 가장 높게 측정되는 팀을 가려내는 대회를 진행.  
`YSCB Workload`에 대해 진행했고 1등으로 마무리함.  

- [Tuning contest result](https://github.com/DKU-StarLab/leveldb-study/blob/main/tuning/README.md)
- [Tuning contest report](https://github.com/DKU-StarLab/leveldb-study/blob/main/tuning/%5BTuning%5Dteam_SSTable_report.md)

<br/>  

---------------------  

이하 원본 레포지토리 내용 전문

# LevelDB Study
2022 [DKU System Software Lab](https://sslab.dankook.ac.kr/) LevelDB Study

## Goals
* [Study LevelDB.](./introduction/README.md)
* [Analyze LevelDB](./analysis/README.md)
* Write LevelDB analysis document.
  * github: https://github.com/DKU-StarLab/leveldb-wiki
  * website: https://sslab.dankook.ac.kr/leveldb-wiki
* [Tuning Contest: YCSB](./tuning/README.md)
* Write a research paper on what you learned.

## Members
* Student:
  - 1.WAL/Manifest: [Isu Kim](https://github.com/gooday2die), [Seyeon Park ](https://github.com/SayOny), [Suhwan Shin](https://github.com/Student5421)
  - 2.Memtable: [Taegyu Cho ](https://github.com/HASHTAG-YOU), [Zhu Yongjie](https://github.com/arashio1111)
  - 3.Compaction: [Sangwoo Kang](https://github.com/aarom416), [Seoyoung Park](https://github.com/seo-0), [Zhao Guangxun](https://github.com/ErosBryant)
  - 4.SSTable: [Jongki Park](https://github.com/JongKI-PARK), [Sanghyun Cho](https://github.com/Cho-SangHyun)
  - 5.Bloom Filter: [Hansu Kim](https://github.com/gillyongs)
  - 6.Cache: [Seungwon Ha](https://github.com/ha-seungwon), [Subin Hong](https://github.com/sss654654)
* Assistant: [Min-guk Choi](https://github.com/korea-choi)
* Senior Assistant: [Sounghyoun Lee](https://github.com/shl812), [Hojin Shin](https://github.com/shinhojin)
* Professor: [Jongmoo Choi](http://embedded.dankook.ac.kr/~choijm/), [Seehwan Yoo](https://sites.google.com/site/dkumobileos/members/seehwanyoo)

## Schedule
* Date: Every Tuesday in July, August
* Time: 14:00 ~ 16:00
* Place: Dankook University Software ICT Hall Room 307

|No|Date|Contents|Details|
|--|--|--|--|
Week 1|22-07-05|[Introduction 1](./introduction/README.md)| What is key-value store? </br> Why open-source?|
Week 2|22-07-12|[Introduction 2](./introduction/README.md)|LevelDB Basics </br>LevelDB Install</br>[Homework](https://github.com/DKU-StarLab/leveldb-study/issues/6#issue-1302876982)|
Week 3|22-07-19|[Introduction 3](./introduction/README.md)|Analysis Tools</br>[Homework Solutions](./homework/README.md)|
|Week 4|22-07-26|[Benchmark Experiment](./benchmarks/README.md)|LevelDB db_bench|
|Week 5</br>(zoom)|22-08-02|[Analysis 1](./analysis/README.md)|
|Week 6|22-08-09|[Analysis 2](./analysis/README.md)|
|Week 7|22-08-16|[Tuning Contest](./tuning/README.md)|YCSB|
|Week 8|22-08-23|[Analysis 3](./analysis/README.md)|
|Week 9|22-08-31</br>16:00|[Analysis 4](./analysis/README.md)</br>Wrap up|Photo, Dinner|

## Photo
<img src="./photo/photo.png">

## Poster (KOR)
<img src="./photo/poster_kor.png" width="50%">

## References
* Documents
  - [LevelDB Document](https://github.com/google/leveldb/blob/main/doc)
  - [RocksDB Wiki](https://github.com/facebook/rocksdb/wiki)
  - [Jongmoo Choi,『Key-Value Store: Database for Unstructured Bigdata』, 2021](https://github.com/DKU-StarLab/leveldb-study/blob/761b550973ab6d1e88189190e66c0ee19a52aa12/introduction/Jongmoo%20Choi,%20Key-Value%20Store%20-%20Database%20for%20Unstructured%20Bigdata,%202021.pdf)
  - [Fenggang Wu, 『LevelDB Introduction』, 2016](https://www-users.cselabs.umn.edu/classes/Spring-2020/csci5980/index.php?page=presentation)
  - [rjl493456442, 『leveldb-handbook (CHS)』, 2022](https://leveldb-handbook.readthedocs.io/zh/latest/)
  - [rsy56640, 『read_and_analyse_levelDB (CHS)』](https://github.com/rsy56640/read_and_analyse_levelDB/tree/master/reference)
  - [FOCUS,『LevelDB fully parsed (CHS)』](https://www.zhihu.com/column/c_1258068131073183744)
  - [bloomingTony, 『Research on Network and Storage Technology(CHS)』](https://www.zhihu.com/column/c_180212366)
  - [木鸟杂记,『Talking about LevelDB data structure (CHS)』, 2021 ](https://www.qtmuniao.com/categories/%E6%BA%90%E7%A0%81%E9%98%85%E8%AF%BB/)
* Lecture
  - [Jongmoo Choi, 『Key-Value Store: Database for Unstructured Bigdata (KOR)』,  2021](https://mooc.dankook.ac.kr/courses/61d537a3b6b71841651153b3)
  - [GL Tech Tutorials, 『LSM trees』, 2021](https://youtube.com/playlist?list=PLRNjlOFk-f0lJJZVoSAmcwZgVtp64tXaX)
  - [Wei Zhou, LevelDB YouTube playlist](https://youtube.com/playlist?list=PLaCN8MYUet0tR1xn5d8ZtCumHKtP6Wkeq)
* Analysis Tools
  - [GDB](https://www.sourceware.org/gdb/)
  - [Understand](https://licensing.scitools.com/download)
  - [Uftrace](https://github.com/namhyung/uftrace)
* Previous Study
  - [DKU RocksDB Festival, 2021](https://github.com/DKU-StarLab/RocksDB_Festival)
  

## File
- [presentation format](./file/%5Bformat%5Dleveldb_study_ppt.pptx)
- [kcc research paper format](./file/%5Bformat%5Dresearch_paper(KCC).hwp)
- [leveldb db_bench script](./file/bench_script.sh)
- [leveldb uftrace script](./file/uftrace_script.sh)
