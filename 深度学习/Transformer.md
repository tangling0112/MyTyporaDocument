# LSTM

```python
'''包的导入'''
import torch.nn.functional as F
import torch
from torch import dot as dot
from torch.nn import Parameter
from torch import Tensor
import torch.nn as nn
import torch.optim as optim
from torch.nn import init
import math
'''LSTM模型的建立'''
class NaiveLSTM(nn.Module):
    def __init__(self, input_size: int, hidden_size: int):
        '''
        :param input_size_:输入的词词向量的长度
        :param hidden_size:隐层单元的长度
        '''
        super(NaiveLSTM, self).__init__()
        self.input_size = input_size
        self.hidden_size = hidden_size
        
		'''通过torch.nn.Parameter函数来给模型注册参数'''
        #注意每个权重都是具备三个维度的，第一个维度用于表示Batch
        # 输入门的权重矩阵和bias矩阵
        self.w_ii = Parameter(Tensor(1,input_size,hidden_size))
        self.w_hi = Parameter(Tensor(1,hidden_size, hidden_size))
        self.b_ii = Parameter(Tensor(1,1,hidden_size))
        self.b_hi = Parameter(Tensor(1,1,hidden_size))
        # 遗忘门的权重矩阵和bias矩阵
        self.w_if = Parameter(Tensor(1,input_size,hidden_size))
        self.w_hf = Parameter(Tensor(1,hidden_size, hidden_size))
        self.b_if = Parameter(Tensor(1,hidden_size))
        self.b_hf = Parameter(Tensor(1,hidden_size))
        # 输出门的权重矩阵和bias矩阵
        self.w_io = Parameter(Tensor(1,input_size,hidden_size))
        self.w_ho = Parameter(Tensor(1,hidden_size, hidden_size))
        self.b_io = Parameter(Tensor(1,1,hidden_size))
        self.b_ho = Parameter(Tensor(1,1,hidden_size))
        # cell的的权重矩阵和bias矩阵
        self.w_ig = Parameter(Tensor(1,input_size,hidden_size))
        self.w_hg = Parameter(Tensor(1,hidden_size, hidden_size))
        self.b_ig = Parameter(Tensor(1,1,hidden_size))
        self.b_hg = Parameter(Tensor(1,1,hidden_size))

        #self.reset_weigths()
	#类内权重初始化函数
    def reset_weigths(self):
        stdv = 1.0 / math.sqrt(self.hidden_size)
        for weight in self.parameters():
            init.uniform_(weight, -stdv, stdv)
            
	#前向传播函数
    def forward(self, inputs, state=None):
        '''
        :param inputs:输入的[B,seq_len,wordvector]型三维数组
        :param state:一个用于指定初始的H0与C0的元组，当不传入时，会由模型自动初始化H0与C0
        '''
        
        #初始化隐藏状态H0与记忆细胞C0
        if state is None:
            h_t = torch.zeros(1, self.hidden_size)
            c_t = torch.zeros(1, self.hidden_size)
        else:
            (h, c) = state
            h_t = h.squeeze(0)
            c_t = c
            
        hidden_seq = []#用于存储每一个隐藏状态
     
        seq_size = inputs.shape[1]#获取SeqLen，如一个句子有10个词，那么seq_size=10
        
        for t in range(seq_size):#按照词进行迭代
            #迭代第t个词
            x = inputs[:, t, :].unsqueeze(1)
            '''
            [batch,1,wordvector]@[1,wordvector,hiddensize]+
            [1,1,hiddensize]+
            [1,1,hiddensize]@[batch,hiddensize,hiddensize]+
            [1,1,hiddensize]
            =[batch,1,hiddensize]
            '''
            # 输入门
            i = torch.sigmoid(x@self.w_ii + self.b_ii + h_t@self.w_hi  + self.b_hi)
            # 遗忘门
            f = torch.sigmoid(x@self.w_if + self.b_if + h_t@self.w_hf + self.b_hf)
            # 记忆细胞
            g = torch.tanh(x@self.w_ig + self.b_ig + h_t@self.w_hg + self.b_hg)
            # 输出门
            o = torch.sigmoid(x@self.w_io + self.b_io + h_t@self.w_ho + self.b_ho)
            #由于激活函数不需要更新参数,因此我们没有使用nn.sigmoid()类而是直接使用函数形式激活函数
            
            '''
            [batch,1,hiddensize]*[batch,1,hiddensize]+
            [batch,1,hiddensize]*[batch,1,hiddensize]
            =[batch,1,hiddensize]
            '''
            #记忆细胞更新
            c_next = f * c_t + i * g
           
            #[batch,1,hiddensize]*[batch,1,hiddensize]==>[batch,1,hiddensize]
            #隐藏状态更新
            h_next = o * torch.tanh(c_next)
            #记忆细胞与隐藏细胞维度填充
            c_next_t = c_next
            h_next_t = h_next
            hidden_seq.append(h_next_t)

        #存储下所有隐藏状态，在NLP中则相当于存储每一个词的隐藏状态
        #stack([batch,1,wordvector])=>[seqlen,batch,1,wordvector]
        hidden_seq = torch.stack(hidden_seq, dim=0)
        
        #return [seqlen,batch,1,wordvector],([batch,1,hiddensize],[batch,1,hiddensize])
        return hidden_seq, (h_next_t, c_next_t)
#模型参数初始化函数
def reset_weigths(model):
    '''
    :param model:一个实例化的LSTM模型
    '''
    for weight in model.parameters():
        init.constant_(weight,0.4)
        
#参数的三个维度为[BatchSize,SeqLen,WordVectorLen]
inputs = torch.ones(24, 10, 10)
h0 = torch.ones(24, 1, 30)
c0 = torch.ones(24, 1, 30)

#实例化一个inputsize=10，hiddensize=20的LSTM类
#model = NaiveLSTM(10, 30)

#初始化模型权重
#reset_weigths(naive_lstm)
#output1, (hn1, cn1) = naive_lstm(inputs, (h0, c0))
#print(hn1.shape, cn1.shape, output1.shape)
#print(output1)
device=torch.device('cuda')

#实例化一个inputsize=10，hiddensize=20的LSTM类，并复制到GPU上
model = NaiveLSTM(10, 30).to(device)
#初始化模型权重
reset_weigths(model)
#定义损失函数
LossFunction=nn.MSELoss(model.parameters()).to(device)

print(model.parameters().__next__())


```



# Transformer

## 1 自定义英中数据集

#### 1.1 定义文本标识符

```python
pad_token = "<pad>"#用于填充字符串从而让每一个输入等长
unk_token = "<unk>"#用于当我们的单词没有存在我们的词典中时，标志该单词为未知单词
bos_token = "<bos>"#用于标识句子的开始
eos_token = "<eos>"#用于标识句子的结束

extra_tokens = [pad_token, unk_token, bos_token, eos_token]

PAD = extra_tokens.index(pad_token)
UNK = extra_tokens.index(unk_token)
BOS = extra_tokens.index(bos_token)
EOS = extra_tokens.index(eos_token)
```

#### 1.2 定义单词转化索引函数

```python
def convert_text2idx(self, examples, word2idx):
    """
    :param examples: 一个容纳了数据集内所有句子的二维数组,每一行为一个句子
    :param word2idx: 数据集的索引形式表示的二维数组
    :return:一个数据集的索引化表示的二维数组，每一行为一个句子w
    """
    
    idx_text = []  # 用于容纳整个文本数据集的用词典索引替代单词的二维数组,每一行为数据集中的一句话
    idx_line = []  # 用于容纳单个句子的用词典索引代替单词的一维数组
    for sent in examples:
        idx_line.append(self.BOS)  # 在句子首部添加BOS开始符号
        for w in sent:
            if w in word2idx:
                idx_line.append(word2idx[w])
            else:
                idx_line.append(self.UNK)
        idx_line.append(self.EOS)  # 在句子尾部添加EOS句子结束符号
        idx_text.append(idx_line)
        idx_line = []
    return idx_text

```

#### 1.3 定义索引转化单词函数

```python
def convert_idx2en_text(self, example, idx2word):
    """
    :param example: 一个有单词索引组成的一维数组
    :param idx2word:
    :return: 一个字符串
    """
    
    words = []
    for i in example:
        if i == self.EOS:  # 当遇到结束标识符时终止
            break
        if i == self.BOS:
            continue
        words.append(idx2word[i])  # 将该索引对应的单词
    return ' '.join(words)  # 将单词数组以每个元素之间相隔一个空白符的规则转化为字符串
```

#### 1.4 定义`padding`添加函数

```python
def add_padding(self, idx_text, max_len):
    """
    :param idx_text: 已经转化为词典索引标识的代表数据集的二维数组,每一行表示一个句子
    :param max_len: 指示最大句子包含词语数,如果小于该值,则添加一定个数PAD符号填充到max_len长度
    :return: 添加了PAD之后的数据集的索引形式二维数组
    """
    
    PADadded_idx_text = []
    for idx_line in idx_text:
        if len(idx_line) == max_len:
            continue
        elif len(idx_line) < max_len:
            for i in range(max_len - len(idx_line)):
                idx_line.append(self.PAD)
        PADadded_idx_text.append(idx_line)
    return PADadded_idx_text  # 已经添加了PAD的代表数据集的二维索引值数组
```



#### 1.5 定义英文数据集读取函数

```python
def read_en_corpus(self, src_path, max_len, lower_case=False):
    """
    :param src_path: 英文数据集保存的文件地址
    :param max_len: 读取的一句话可以包含的最多单词数
    :param lower_case: 用于指定是否将文本转化为小写
    :return: 英文数据集的二维数组
    """
    
    src_sents = []
    empty_lines, exceed_lines = 0, 0
    with open(src_path,encoding='utf8') as src_file:
        for idx, src_line in enumerate(src_file):
            if idx == 10000:
                break
            if src_line.strip() == '':  # 当遇到空行时，让empty_lines加1并直接跳过这一行
                empty_lines += 1
                continue
            if lower_case:  # 用于对文本进行小写转换
                src_line = src_line.lower()

            src_words = src_line.strip().split()
            if max_len is not None and len(src_words) > max_len:  # 判断指定行包含的单词数量是否超过max_len如果超过则跳过该行，不将其录入
                exceed_lines += 1
                continue
            src_sents.append(src_words)  # 添加读取到的行到src_sents数组中形成二维数组
    return src_sents
```

#### 1.6 定义中文数据集读取函数

```python
import jieba
def read_zh_corpus(self, src_path, max_len):
    """
    :param src_path: 中文数据集保存的文件地址
    :param max_len: 读取的一句话可以包含的最多单词数
    :return: 中文数据集的二维数组
    """
    
    src_sents = []
    empty_lines, exceed_lines = 0, 0
    with open(src_path,encoding='utf8') as src_file:
        for idx, src_line in enumerate(src_file):
            if idx == 10000:
                break
            if src_line.strip() == '':  # 当遇到空行时，让empty_lines加1并直接跳过这一行
                empty_lines += 1
                continue
            src_words = jieba.lcut(src_line.strip())  # 使用jieba库的精确搜索模式对句子进行词语拆分
            if max_len is not None and len(src_words) > max_len:  # 判断指定行包含的单词数量是否超过max_len如果超过则跳过该行，不将其录入
                exceed_lines += 1
                continue
            src_sents.append(src_words)  # 添加读取到的行到src_sents数组中形成二维数组
    return src_sents
```

#### 1.7 定义英文单词词典建立函数

```python
from collections import Counter
def build_En_vocab(self, examples, max_size, min_freq, extra_tokens):
    """
    :param examples: 一个包含着英文数据集内所有文本的二维数组,每一行,为一句话
    :param max_size: 定义词典最大可包含的单词的数量
    :param min_freq: 用于指定一个单词的最小出现次数,如果小于该值,则这个词不会被添加到字典中
    :param extra_tokens: 一个存储着四个标识符的数组
    :return: 我们的counter实例
        	,单词为键,单词在idx2word中的索引为值的字典对象
        	,保存了词典中所有单词的二维数组,每一行为一个单词
    """
    
    counter = Counter()  # 一个计数器实例,来自于Counter库
    word2idx, idx2word = {}, []
    '''将四个标识符添加到字典中'''
    if extra_tokens:
        idx2word += extra_tokens
        word2idx = {word: idx for idx, word in enumerate(extra_tokens)}

    min_freq = max(min_freq, 1)
    max_size = max_size + len(idx2word) if max_size else None  # 由于扩展到四个标识符不算到词典最大长度中,因此将max_size扩大
    for sent in examples:  # 迭代examples数组的每一行,也即数据集的每一句话.
        for w in sent:  # 迭代sent数组的每一个元素,也即一句话中的每一个单词
            counter.update([w])
    sorted_counter = sorted(counter.items(), key=lambda tup: tup[0])  # 按照单词词序进行排列
    '''
    每一个counter的item都是一个含有两个元素的数组,第一个元素指的是被添加到counter实例的单词,第二个为其出现的次数
    '''
    sorted_counter.sort(key=lambda tup: tup[1], reverse=True)  # 再按照单词出现的频率从大到小排序

    for word, freq in sorted_counter:
        if freq < min_freq or (max_size and len(idx2word) == max_size):
            break

        idx2word.append(word)  # 将单词添加到idx2word数组中
        word2idx[word] = len(idx2word) - 1
        '''
        1.idx2word数组中添加一个单词,此时长度为n+1
        2.在word2idx字典中以单词字符串为键,该单词在idx2word中的索引值为值添加
        '''
    return counter, word2idx, idx2word
```

#### 1.8 借助`jieba`库定义中文词语词典建立函数

```python
from collections import Counter
import jieba
def build_zh_vocab(self, examples, max_size, min_freq, extra_tokens):
    """
    :param examples: 一个包含着中文数据集内所有文本的二维数组,每一行,为一句话
    :param max_size: 定义词典最大可包含的词语的数量
    :param min_freq: 用于指定一个词语的最小出现次数,如果小于该值,则这个词不会被添加到词典中
    :param extra_tokens: 一个存储着四个标识符的数组
    :return:我们的counter实例
           ,词为键,单词在idx2word中的索引为值的字典对象
           ,保存了词典中所有单词的二维数组,每一行为一个单词
    """
    
    counter = Counter()  # 一个计数器实例,来自于Counter库
    word2idx, idx2word = {}, []
    '''将四个标识符添加到字典中'''
    if extra_tokens:
        idx2word += extra_tokens
        word2idx = {word: idx for idx, word in enumerate(extra_tokens)}

    min_freq = max(min_freq, 1)
    max_size = max_size + len(idx2word) if max_size else None  # 由于扩展到四个标识符不算到词典最大长度中,因此将max_size扩大
        for sent in examples:  # 迭代examples数组的每一行,也即数据集的每一句话.
        for w in sent:  # 迭代sent数组的每一个元素,也即一句话中的每一个单词
            counter.update([w])
    sorted_counter = sorted(counter.items(), key=lambda tup: tup[0])  # 按照单词词序进行排列
    '''
    每一个counter的item都是一个含有两个元素的数组,第一个元素指的是被添加到counter实例的单词,第二个为其出现的次数
    '''
    sorted_counter.sort(key=lambda tup: tup[1], reverse=True)  # 再按照单词出现的频率从大到小排序

    for word, freq in sorted_counter:
        if freq < min_freq or (max_size and len(idx2word) == max_size):
            break

        idx2word.append(word)  # 将单词添加到idx2word数组中
        word2idx[word] = len(idx2word) - 1
        '''
        1.idx2word数组中添加一个单词,此时长度为n+1
        2.在word2idx字典中以单词字符串为键,该单词在idx2word中的索引值为值添加
        '''
    return counter, word2idx, idx2word
```



#### 1.9 定义数据集类

```python
from torch.utils.data import Dataset
class MyDataset(Dataset):
    def __init__(self, path_en,path_zh, max_len, max_size, min_freq, training='train'):
        """
        :param path_en: 英语数据集的保存位置
        :param path_zh: 中文数据集的保存位置
        :param max_len: 句子的最大单词数
        :param max_size: 词典的最大容纳量
        :param min_freq: 词语出现的最小次数，少于该次数的词语将不会被记入词典
        :param training: 用于指定数据获取的方式
        """

        '''定义文本标识符'''
        pad_token = "<pad>"  # 用于填充字符串从而让每一个输入等长
        unk_token = "<unk>"  # 用于当我们的单词没有存在我们的词典中时，标志该单词为未知单词
        bos_token = "<bos>"  # 用于标识句子的开始
        eos_token = "<eos>"  # 用于标识句子的结束

        self.extra_tokens = [pad_token, unk_token, bos_token, eos_token]

        self.PAD = self.extra_tokens.index(pad_token)
        self.UNK = self.extra_tokens.index(unk_token)
        self.BOS = self.extra_tokens.index(bos_token)
        self.EOS = self.extra_tokens.index(eos_token)

        self.path_en = path_en
        self.path_zh = path_zh
        self.max_len = max_len
        self.max_size = max_size
        self.min_freq = min_freq

        '''读取文件获取数据集组成的二维向量'''
        self.examples_zh = self.read_zh_corpus(self.path_zh, self.max_len)
        self.examples_en = self.read_en_corpus(self.path_en, self.max_len)
        '''使用数据集的二维向量构建词典'''
        self.counter_en, self.word2idx_en, self.idx2word_en = self.build_En_vocab(
            self.examples_en, self.max_size, self.min_freq, self.extra_tokens)
        self.counter_zh, self.word2idx_zh, self.idx2word_zh = self.build_zh_vocab(
            self.examples_zh, self.max_size, self.min_freq, self.extra_tokens)
        '''
        word2idx_en:英文的词语转索引,是一个字典对象
        word2idx_zh:中文的词语转索引,是一个字典对象
        idx2word_en:英文的索引转词语,是一个列表对象
        idx2word_zh:中文的索引转词语,是一个列表对象
        '''
        '''使用词典将原数据集组成的二维向量转化成由字典索引组成的二维向量'''
        self.examples_en_idx = self.convert_text2idx(self.examples_en, self.word2idx_en)
        self.examples_zh_idx = self.convert_text2idx(self.examples_zh, self.word2idx_zh)
        
        '''对索引形式的数据集的二维数组表示添加PAD标识符,从而让每一个句子等长,如果不这样,则会由于句子不等长组不成[B,L]的标准形式的Tensor二维数组'''
        self.examples_en_idx = self.add_padding(self.examples_en_idx,self.max_len)
        self.examples_zh_idx = self.add_padding(self.examples_zh_idx, self.max_len)
        
        self.vocab_size_zh = len(self.idx2word_zh)
        self.vocab_size_en = len(self.idx2word_en)

        if training == 'train':
            self.en_text_idx = self.examples_en_idx[0:6000]
            self.zh_text_idx = self.examples_zh_idx[0:6000]
        elif training == 'valid':
            self.en_text_idx = self.examples_en_idx[6001:8000]
            self.zh_text_idx = self.examples_zh_idx[6001:8000]
        elif training == 'test':
            self.en_text_idx = self.examples_en_idx[8001:10000]
            self.zh_text_idx = self.examples_zh_idx[8001:10000]

    def __len__(self):
        return len(self.en_text_idx)

    def __getitem__(self, idx):
        return self.en_text_idx[idx],self.zh_text_idx[idx]
    
    def get_idx2word(self):
        return self.idx2word_en,self.idx2word_zh
    
    def get_word2idx(self):
        return self.word2idx_en,self.word2idx_zh
```

#### 1.10 注意

- 数据集的输出为`[L]`一个一维数组，其长度固定为64，实际句子长度小于64，我们通过补`PAD=0`使其等长,从而便于形成`[B,L]`的标准形式.
- 数据集的每一个输出的第一个元素都是`BOS`的词典索引,而句子的结尾为`EOS`的词典索引,再之后为`PAD`的词典索引
- 我们可以使用数据集的`get_vocab_szie()`方法获得==(英文词典大小,中文词典大小)==的元组形式返回值
- 我们可以通过数据集的`get_word2idx()`方法获得==(英文单词转索引词典`word2idx_en`,中文词语转索引词典`word2idx_zh`)==的元组形式返回值
- 我们可以通过数据集的`get_idx2word()`方法获得==(英文单词列表`idx2word_en`,中文单词列表`idx2word_zh`)==的元组形式返回值
- 我们的英文索引型句子可以通过数据集的`convert_idx2en_text( example, idx2word)`方法转化为句子的原始形式

- 当使用DataLoader加载我们的数据集时，当`batch_size`大于一时，我们的数据集的输出为如下形式，以batch_size=2为例

  ```python
  [tensor[1,2],tensor[3,4],tensor[5,6]]
  ```

- 我们可以使用如下方法让其变成标准的`[B,L]`形式

  ```
  input = [element.tolist() for element in input]
  input = torch.tensor(input)
  #input = [L,B]
  input = input.t()
  #input = [B,L]
  ```
  

## 2 自定义`Transformer`模型

### 2.1 定义子模块

#### 2.1 Input/Output Embedding

```
这一步与Decoder与Encoder模块中被定义,并没有单独进行
```



#### 2.2 Padding_mask

```python
def padding_mask(seq_k, seq_q):
	# seq_k和seq_q的形状都是[B,L]
    len_q = seq_q.size(1)
    # `PAD` is 0
    pad_mask = seq_k.eq(0)
    pad_mask = pad_mask.unsqueeze(1).expand(-1, len_q, -1)  # shape [B, L_q, L_k]
    #pad_mask是一个布尔值组成的三维数组seq_k等于0的位置为True
    return pad_mask
```

#### 2.4 Seqence_mask

```python
import torch
def sequence_mask(seq):
    batch_size, seq_len = seq.size()
    mask = torch.triu(torch.ones((seq_len, seq_len), dtype=torch.uint8),
                    diagonal=1)
    mask = mask.unsqueeze(0).expand(batch_size, -1, -1)  # [B, L, L]
    #mask是一个0，1三维数组由B个[L,L]的二维数组堆叠而成，[L,L]二维数组为一个上三角全部为1的上三角矩阵
    return mask
```

#### 2.4 ScaledDotProductAttention

```python
import torch
import torch.nn as nn
class ScaledDotProductAttention(nn.Module):
    """Scaled dot-product attention mechanism."""

    def __init__(self, attention_dropout=0.0):
        super(ScaledDotProductAttention, self).__init__()
        self.dropout = nn.Dropout(attention_dropout)
        self.softmax = nn.Softmax(dim=2)

    def forward(self,Q, K, V, dk=None, attn_mask=None):
        '''
        
        :param Q: Queries张量，形状为[B, L_q, D_q]
        :param K: Keys张量，形状为[B, L_k, D_k]
        :param V: Values张量，形状为[B, L_v, D_v]，一般来说就是k
        :param dk: 缩放因子，一个浮点标量
        :param attn_mask: Masking张量，形状为[B, L_q, L_k]
        :return: 上下文张量和attetention张量
        '''

        attention = torch.bmm(Q, K.transpose(1, 2))
        if dk:
            attention = attention * dk
        if attn_mask !=None:
        # 给需要mask的地方设置一个负无穷
        	attention = attention.masked_fill_(attn_mask, -99999999)
# 计算softmax
        attention = self.softmax(attention)
# 添加dropout
        attention = self.dropout(attention)
# 和V做点积
        context = torch.bmm(attention, V)
        return context, attention

```

#### 2.2 Position Embeding

```python
import torch
import torch.nn as nn
import numpy as np

class PositionalEncoding(nn.Module):

    def __init__(self, d_model, max_seq_len):
        '''
        :param d_model: 一个标量。每个词语的词向量长度，论文默认是512
        :param max_seq_len: 指定一个句子的最大长度，在我们的数据集里为64
        '''
        
        super(PositionalEncoding, self).__init__()

        # 根据论文给的公式，构造出PE矩阵
        position_encoding = torch.tensor(np.array([
            [pos / np.power(10000, 2.0 * (j // 2) / d_model) for j in range(d_model)]
            for pos in range(max_seq_len)]))
        # 偶数列使用sin，奇数列使用cos
        position_encoding[:, 0::2] = torch.sin(position_encoding[:, 0::2])
        position_encoding[:, 1::2] = torch.cos(position_encoding[:, 1::2])

        # 在PE矩阵的第一行，加上一行全是0的向量，代表这`PAD`的positional encoding
        # 在word embedding中也经常会加上`UNK`，代表位置单词的word embedding，两者十分类似
        # 那么为什么需要这个额外的PAD的编码呢？很简单，因为文本序列的长度不一，我们需要对齐，
        # 短的序列我们使用0在结尾补全，我们也需要这些补全位置的编码，也就是`PAD`对应的位置编码
        pad_row = torch.zeros([1, d_model])
        position_encoding = torch.cat((pad_row, position_encoding))

        # 嵌入操作，+1是因为增加了`PAD`这个补全位置的编码，
        # Word embedding中如果词典增加`UNK`，我们也需要+1。看吧，两者十分相似
        self.position_encoding = nn.Embedding(max_seq_len + 1, d_model)
        self.position_encoding.weight = nn.Parameter(position_encoding,
                                                     requires_grad=False)

    def forward(self, input_len):
        '''
        :param input_len: 一个张量，形状为[BATCH_SIZE, 1]。每一个张量的值代表这一批文本序列中对应的长度。
        :return:返回这一批序列的位置编码，进行了对齐。
        '''
        # 找出这一批序列的最大长度
        #max_len = torch.max(input_len)
        #tensor = torch.cuda.LongTensor if input_len.is_cuda else torch.LongTensor
        # 对每一个序列的位置进行对齐，在原序列位置的后面补上0
        # 这里range从1开始也是因为要避开PAD(0)的位置
        #input_pos = tensor(
            #[list(range(1, len + 1)) + [0] * (max_len - len) for len in input_len])
        return self.position_encoding(input_len)
```



#### 2.3 MultiHeadAttention

```python
import torch
import torch.nn as nn
import ScaledDotProductAttention

class MultiHeadAttention(nn.Module):

    def __init__(self, model_dim=512, num_heads=8, dropout=0.0):
        super(MultiHeadAttention, self).__init__()

        self.dim_per_head = model_dim // num_heads
        self.num_heads = num_heads
        self.linear_k = nn.Linear(model_dim, self.dim_per_head * num_heads)
        self.linear_v = nn.Linear(model_dim, self.dim_per_head * num_heads)
        self.linear_q = nn.Linear(model_dim, self.dim_per_head * num_heads)

        self.dot_product_attention = ScaledDotProductAttention.ScaledDotProductAttention(dropout)
        self.linear_final = nn.Linear(model_dim, model_dim)
        self.dropout = nn.Dropout(dropout)
		# multi-head attention之后需要做layer norm
        self.layer_norm = nn.LayerNorm(model_dim)

    def forward(self, key, value, query, attn_mask=None):
		# 残差连接
        residual = query

        dim_per_head = self.dim_per_head
        num_heads = self.num_heads
        batch_size = key.size(0)

        # linear projection
        key = self.linear_k(key)
        value = self.linear_v(value)
        query = self.linear_q(query)

        # split by heads
        key = key.view(batch_size * num_heads, -1, dim_per_head)
        value = value.view(batch_size * num_heads, -1, dim_per_head)
        query = query.view(batch_size * num_heads, -1, dim_per_head)

        if attn_mask:
            attn_mask = attn_mask.repeat(num_heads, 1, 1)
        # scaled dot product attention
        scale = (key.size(-1) // num_heads) ** -0.5
        context, attention = self.dot_product_attention(
          query, key, value, scale, attn_mask)

        # concat heads
        context = context.view(batch_size, -1, dim_per_head * num_heads)

        #进行最终的线性层处理
        output = self.linear_final(context)

        #实现Dropout
        output = self.dropout(output)

        # 实现ADD&Norm层
        output = self.layer_norm(residual + output)

        return output, attention
```



#### 2.4 Add&Norm

#### 2.5 FeedForward

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class PositionalWiseFeedForward(nn.Module):

    def __init__(self, model_dim=512, ffn_dim=2048, dropout=0.0):
        super(PositionalWiseFeedForward, self).__init__()
        self.w1 = nn.Conv1d(model_dim, ffn_dim, 1)
        self.w2 = nn.Conv1d(model_dim, ffn_dim, 1)
        self.dropout = nn.Dropout(dropout)
        self.layer_norm = nn.LayerNorm(model_dim)

    def forward(self, x):
        output = x.transpose(1, 2)
        output = self.w2(F.relu(self.w1(output)))
        output = self.dropout(output.transpose(1, 2))

        # add residual and norm layer
        output = self.layer_norm(x + output)
        return output
```



#### 2.6 Masked MultiHeadAttention

#### 2.7 EncoderLayer

```python
import torch
import torch.nn as nn
import MultiHeadSelfAttention
import PositionalWiseFeedForward
import MultiHeadSelfAttention
import padding_mask
class EncoderLayer(nn.Module):
	##Encoder的一层。
    def __init__(self, model_dim=512, num_heads=8, ffn_dim=2048, dropout=0.0):
        super(EncoderLayer, self).__init__()
        self.attention = MultiHeadSelfAttention.MultiHeadAttention(model_dim, num_heads, dropout)
        self.feed_forward = PositionalWiseFeedForward.PositionalWiseFeedForward(model_dim, ffn_dim, dropout)
    def forward(self, inputs, attn_mask=None):

        # self attention
        context, attention = self.attention(inputs, inputs, inputs, padding_mask)

        # feed forward network
        output = self.feed_forward(context)

        return output, attention

```



#### 2.8 DecoderLayer

```python
import PositionalWiseFeedForward
class DecoderLayer(nn.Module):

    def __init__(self, model_dim, num_heads=8, ffn_dim=2048, dropout=0.0):
        super(DecoderLayer, self).__init__()

        self.attention = MultiHeadSelfAttention.MultiHeadAttention(model_dim, num_heads, dropout)
        self.feed_forward = PositionalWiseFeedForward.PositionalWiseFeedForward(model_dim, ffn_dim, dropout)

    def forward(self,
              dec_inputs,
              enc_outputs,
              self_attn_mask=None,
              context_attn_mask=None):
        # self attention, all inputs are decoder inputs
        dec_output, self_attention = self.attention(
          dec_inputs, dec_inputs, dec_inputs, self_attn_mask)

        # context attention
        # query is decoder's outputs, key and value are encoder's inputs
        dec_output, context_attention = self.attention(
          enc_outputs, enc_outputs, dec_output, context_attn_mask)

        # decoder's output, or context
        dec_output = self.feed_forward(dec_output)

        return dec_output, self_attention, context_attention
```



#### 2.9 Encoder

```python
import torch
import torch.nn as nn
import EncoderLayer
import PositionalWiseFeedForward
import PositionEncoding
import padding_mask
class Encoder(nn.Module):
	#多层EncoderLayer组成Encoder。
    def __init__(self,
               vocab_size,
               max_seq_len,
               num_layers=6,
               model_dim=512,
               num_heads=8,
               ffn_dim=2048,
               dropout=0.0):
        super(Encoder, self).__init__()

        self.encoder_layers = nn.ModuleList(
          [EncoderLayer.EncoderLayer(model_dim, num_heads, ffn_dim, dropout) for _ in
           range(num_layers)])

        self.seq_embedding = nn.Embedding(vocab_size, model_dim, padding_idx=0)
        self.pos_embedding = PositionEncoding.PositionalEncoding(model_dim, max_seq_len)

    def forward(self, inputs, inputs_len):
        #词语词向量化
        output = self.seq_embedding(inputs)
        #进行PositionEmbeding
        output += self.pos_embedding(inputs_len)

        self_attention_mask = padding_mask.padding_mask(inputs, inputs)

        attentions = []
        for encoder in self.encoder_layers:
            output, attention = encoder(output, self_attention_mask)
            attentions.append(attention)

        return output, attentions
```



#### 2.10 Decoder

```python
import torch.nn as nn
import DecoderLayer
import PositionEncoding
import padding_mask
import sequence_mask
import torch
class Decoder(nn.Module):

    def __init__(self,
               vocab_size,
               max_seq_len,
               num_layers=6,
               model_dim=512,
               num_heads=8,
               ffn_dim=2048,
               dropout=0.0):
          '''
        :param vocab_size: 输入数据集的词典包含的词语的个数
        :param max_seq_len: 输入数据集单个句子的最大长度
        :param num_layers: 定义使用多少层编码器解码器叠加，在Transformer的论文中使用了6层
        :param model_dim: 指定单个词的词向量的长度
        :param num_heads: MultiHeadAttention的头数
        :param ffn_dim: 全连接层的输出长度
        :param dropout: Dropout机制的发生概率
        '''
        super(Decoder, self).__init__()

        self.num_layers = num_layers
        
		#实例化num_layers个DecoderLayer
        self.decoder_layers = nn.ModuleList(
          [DecoderLayer.DecoderLayer(model_dim, num_heads, ffn_dim, dropout) for _ in
           range(num_layers)])
        
		#实例化词语词向量化类
        self.seq_embedding = nn.Embedding(vocab_size, model_dim, padding_idx=0)
        #实例化PositionEmbeding类
        self.pos_embedding = PositionEncoding.PositionalEncoding(model_dim, max_seq_len)

    def forward(self, inputs, inputs_len, enc_output, context_attn_mask=None):
        '''
        :param inputs:[B,L]形式的二维数组
        :param input_len:输入句子的长度
        :param enc_output:编码器的输出
        :param context_attn_mask:用于做Mask的数组，协助MultiHeadAttention实现Masked Multi Head Attention
        '''
        
        #对tgt_seq进行词向量化
        output = self.seq_embedding(inputs)
        #对tgt_len进行Position_embeding
        output += self.pos_embedding(inputs_len)

        '''实现masked,从而配合MultiHeadAttention实现Masked MultiHeadAttention'''
        self_attention_padding_mask = padding_mask.padding_mask(inputs, inputs)
        seq_mask = sequence_mask.sequence_mask(inputs)
        
        #以(self_attention_padding_mask + seq_mask)每一个元素与0作比较.若大于0则取1,否则取0
        self_attn_mask = torch.gt((self_attention_padding_mask + seq_mask), 0)

        self_attentions = []
        context_attentions = []
        for decoder in self.decoder_layers:
            output, self_attn, context_attn = decoder(
            output, enc_output, self_attn_mask, context_attn_mask)
            self_attentions.append(self_attn)
            context_attentions.append(context_attn)

        return output, self_attentions, context_attentions
```



### 2定义主模块

```python
import torch.nn.functional as F
import torch
from torch import dot as dot
from torch.nn import Parameter
from torch import Tensor
import torch.nn as nn
import torch.optim as optim
from torch.nn import init
import math
import numpy as np
import torch
import Encoder
import Decoder
import padding_mask

class Transformer(nn.Module):
    def __init__(self,
               src_vocab_size,
               src_max_len,
               tgt_vocab_size,
               tgt_max_len,
               num_layers=6,
               model_dim=512,
               num_heads=8,
               ffn_dim=2048,
               dropout=0.2):
        '''
        
        :param src_vocab_size: 输入数据集的词典包含的词语的个数
        :param src_max_len: 输入数据集单个句子的最大长度
        :param tgt_vocab_size: 输出数据集的词典包含的词语的个数
        :param tgt_max_len: 输出数据集的词典包含的单个句子的最大长度
        :param num_layers: 定义使用多少层编码器解码器叠加，在Transformer的论文中使用了6层
        :param model_dim: 指定单个词的词向量的长度
        :param num_heads: MultiHeadAttention的头数
        :param ffn_dim: 全连接层的输出长度
        :param dropout: Dropout机制的发生概率
        '''
        
        super(Transformer, self).__init__()
		#实例化编码器
        self.encoder = Encoder.Encoder(src_vocab_size, src_max_len, num_layers, model_dim,
                               num_heads, ffn_dim, dropout)
        #实例化解码器
        self.decoder = Decoder.Decoder(tgt_vocab_size, tgt_max_len, num_layers, model_dim,
                               num_heads, ffn_dim, dropout)
		#实例化输出的全连接层
        self.linear = nn.Linear(model_dim, tgt_vocab_size, bias=False)
        #实例化输出的SoftMax层
        self.softmax = nn.Softmax(dim=2)
	#定义前向函数
    def forward(self, src_seq, src_len, tgt_seq, tgt_len):
        '''
        :param src_seq:输入数据集的句子[B,L]
        :param src_len:句子包含的词语的数量[L]
        :param tgt_seq:输出数据集的句子[B,L]
        :param tgt_len:句子包含的词语的数量[L]
        '''
        context_attn_mask = padding_mask.padding_mask(tgt_seq, src_seq)
		#应用解码器
        output, enc_self_attn = self.encoder(src_seq, src_len)

        output, dec_self_attn, ctx_attn = self.decoder(
          tgt_seq, tgt_len, output, context_attn_mask)
		#应用全连接层
        output = self.linear(output)
        #应用SoftMax层
        output = self.softmax(output)

        return output, enc_self_attn, dec_self_attn, ctx_attn

```

### 3 对Transformer进行训练的细节

#### 3.1 注意事项

- 在论文中使用的词嵌入的维度为`512`
- ==训练集的`输入集`的每个句子都应该为`BOS sentence PAD PAD PAD`的形式,`目标集`应该为`sentence EOS PAD PAD PAD`的形式,并且每个句子拥有相同的词语数量(用PAD补充至相同),我们设置为`64`==
- 在训练时的输入为整个目标句子的`Embedding`
- 在训练过程中`Decoder`模块中只有一个`Decoder`组件,其输出即为全部的结果,而在预测过程中则不然,细节将在第四节讲述
- 在训练过程中在`Seq_len=64,Batch_size=1`的假设下,我们的`Decoder`的输出为`[1,64,Vocab_size]`,但是在预测过程中则不然
- 训练时的`Decoder`的`[1,64,Vocab_size]`的输出的第一行并不是指`BOS`这个词的,而是预测得到的`BOS`的下一个词,也就是真实目标句子的第一个词
- 我们的训练使用==交叉熵==函数来计算损失函数
- ==Padding Mask==:在训练的时,我们是以 `batch_size` 为单位的, 那么必然需要添加`PAD`而`PAD`的索引值一般为0,那么就会造成在 Attention 的时候, $QK^T$的对应位置的值为0,而`SoftMax`在$0$上的取值为$0.5$,这就会导致我们的注意力机制给没有任何实际意义的`PAD`分配一个不小的权重,这时不可取的, 因此, 我们将 `PAD` 对应的 $QK^T$ 取值为**负无穷**(可以直接用-999999,主要是不了解使用-np.inf会不会影响torch的自动求导),从而使得`其SoftMax值无限接近与0`从而防止`PAD`被分配到较大权重
- 只要我们的`Batch_size大于1`(在等于1时不需要做`PAD`),那么==Padding Mask==在`Encoder`以及`Decoder`中都需要进行
- ==Sequence Mask==:即在并行化训练时,我们需要对得到的$QK^T$矩阵进行`Mask`以防止排在前面的单词利用排在后面的单词的信息,`Sequence Mask`也是将$QK^T$要`mask`的位置设置为为$-inf$

#### **3.2 并行化训练基本过程**

1. 给`Decoder`输入整个目标句子的`Embedding`表示(在这里为`[1,64,512]`)+`Encoder`的输出`[1,64,512]`
2. 计算`Q,K,V`我们假设$size\ of\quad W_K=W_V=W_Q=[1,512,x]$,那么$size\ of \quad Q,K,V=[1,64,512]×[1,512,x]=[1,64,x]$
3. 计算$QK^T=[1,64,x]×[1,x,64]=[1,64,64]$
4. 进行==Padding Mask==与==Sequence Mask==
5. 由公式计算`Attention`值` [1,64,64]×[1,64,x]=[1,64,x]`
6. 对八个`Attention`输出做`Concat`变成`[1,64,8x]`,==因此由与`Attention`要确保输入与输出形状一致,$x$取值应该为64==
7. 进入第二个`MultiHeadAttention`,==不用加`Mask`==
8. 进入`FeedForWard`
9. 得到输出,进入第二个`Decoder`,==只有第一个Decoder需要加`Mask`,后续的所有Decoder中的第一个`MultiHeadAttention`都不需要进行`Mask`==
10. 依次进入后续`Decoder`(论文中`Decoder`有6个)
11. 最终得到一个`[1,64,512]`的输出,进行==全连接==操作,乘以一个`[1,512,Vocabsize]`的权重矩阵得到`[1,64,Vocab_size]`的输出
12. 通过输出构建出对应的预测目标值的`[1,64,512]`矩阵,然后与实际的`[1,64,512]`矩阵进行`CrossEntropyLoss`求解
    - 当然我们也可以换种思路==将目标值的`[1,64,512]`改造成`[1,64,Vocab_size]`然后进行`CrossEntropyLoss`求解==
13. 进行梯度更新
14. 得到`Decoder`的输出,在`Seq_len=64,词嵌入向量长度=512,Batch_size=1`的假设下,==输出为`[1,64,Vocab_size]`==
15. 这个`[1,64,Vocab_size]`的输出在`sequeez为[64,Vocab_size]`后,第`i`行表示第`i`个位置取词典中各个词的概率

### 4 对Transformer进行测试的细节

#### 4.1 基本过程

1. 给`Decoder`输入`BOS`的`Embedding`表示`[1,1,512]`以及`Encoder`的输出`[1,64,512]`
2. 计算`Q,K,V`,其中`Q`由`Encoder`输出计算,`K,V`由`Embedding`计算$size\ of \quad Q=[1,64,512]×[1,512,x]=[1,64,x]$$size\ of\quad K,V=[1,1,512]×[1,512,x]=[1,1,x]$
3. 计算$QK^T=[1,64,x]×[1,x,1]=[1,64,1]$
4. 做==Padding Mask==
5. 由公式计算得到``Attention`的值`[1,64,1]×[1,1,x]=[1,64,x]`
6. 对八个Attention进行Concat操作`[1,64,x]×8=[1,64,8x]=[1,64,512]`
7. 进入第二个`MultiHeadAttention`,==不用加`Mask`==
8. 进入`FeedForWard`
9. 得到输出,进入第二个`Decoder`,==除第一个需要加`Mask`外,后续的`MultiHeadAttention`不用再进行`Mask`==
10. 依次进入后续`Decoder`(论文中`Decoder`有6个)
11. 最终得到一个`[1,64,512]`的输出,进行==全连接==操作,乘以一个`[1,512,Vocabsize]`的权重矩阵得到`[1,64,Vocab_size]`的输出
12. `[1,64,Vocab_size]`的sequeez为`[64,Vocab_size]`后的第一行就是我们目标序列的第一个位置取值为词典内各个词的概率,当然,剩下的`63`行对于我们的预测而言,没有作用.
13. 由`12`得到第一个位置的概率最高的词的词嵌入向量,并与`BOS`进行`Concat`组成`[1,2,512]`的数组
    - 由`Encoder`输出与以知权重矩阵$W_Q$计算Q,$size\ of \quad Q=[1,64,512]×[1,512,x]=[1,64,x]$
    - 由13得到的`[1,2,512]`的矩阵与已知的权重矩阵$W_K,W_V$进行计算得到$K,V$$size\ of\quad K,V=[1,2,512]×[1,512,x]=[1,2,x]$
    - 计算$QK^T=[1,64,x]×[1,x,2]=[1,64,2]$
    - ==做PaddingMask==
    - 由公式计算得到``Attention`的值`[1,64,2]×[1,2,x]=[1,64,x]`
    - 做Concat得到`[1,64,512]`
    - 重复得到最终输出`[1,64,Vocab_size]`
    - 整理得到第二个词从而构成`[1,3,512]`
14. 用13的输出与Encoder的输出重复2~13,直到遇到预测的词为`EOS`,终止预测过程
15. 按顺序把每一次求得的词按先后顺序进行拼接并去掉`EOS`,便得到了我们的最终的目标句子

#### 4.2 注意事项

1. 在六个`Decoder`中,只有进入第一个`Decoder`的`MultiHeadAttention`之前需要做`Padding Mask`
2. 预测过程中不需要做`Sequence Mask`
3. 没进入一次Decoder模块只可以得到一个预测的目标词,即便输出是`[1,64,Vocab_size]`

## 3 功能实现上的技巧性操作

### 3.1 MultiHeadAttention的一次性完成

### 3.2 Padding Mask的Mask矩阵的生成
