# ijcai-2018
ijcai-2018 top1 solution
一.Graft Learning
首先科普一下什么是嫁接。

在生产实践过程中，嫁接对经济价值的提高，也有着非常多的实例：如普通的水杉，价值一元；而通过嫁接手段，培育成金叶水杉后，经济价值提高20多倍；再如普通的大叶女贞树，价值几角钱；而通过嫁接的手段，培育成彩叶桢树后，其经济价值更高达近百倍。由此可见，嫁接对品种的改良，经济价值的提高都有首非常重要的意义。

影响嫁接成活的主要因素是接穗和砧木的亲和力，其次是嫁接的技术和嫁接后的管理。所谓亲合力，就是接穗和砧木在内部组织结构上、生理和遗传上，彼此相同或相近，从而能互相结合在一起的能力。亲和力高，嫁接成活率高。反之，则成活率低。一般来说，植物亲缘关系越近，则亲和力越强。例如苹果接于沙果；梨接于杜梨、秋子梨；柿接于黑枣；核桃接于核桃楸等亲和力都很好。

大家应该都尝试过只用只用前七天的数据来预测，最后一天来做线下验证，这种效果不太好，但是数据量大，就如同这里的砧木获取容易，但是实际价值不大。
大家应该也尝试过只用最后半天来做预测，达到的效果也已经很不错了，并且比只用前七天的训练好，但是数据量比较少。

那么我们参考嫁接的思想，让砧木来给我们的接穗输送营养。在这里前七天的数据就类似于砧木，后半天的数据类似于接穗。前七天预测出来的结果就是营养。这样我们预测出来的结果也更加接近于最后一天。

那么既然这样我为什么没有使用初赛的数据来做“嫁接“呢？
亲缘关系越近的嫁接成功率越高，在这个问题上也是item相同品别的做这种“嫁接“操作效果才会好，而我们的初赛数据和复赛数据的item并不是一个品类的，所以价值不大。

二.Sample Embedding
sample_emb_x=[x1,x2,x3,x4,...,xn]                   xn为第n个property在不在predict_category_property中
sample_emb_y=[y1,y2,y3,y4,...,yn]                  yn为第n个property在不在item_property_list中

一个user有很多个不同的item交互样本，一个item也有很多不同的user交互样本
user_emb_x=mean([sample_emb_x_1,sample_emb_x_2,...,sample_emb_x_k])         sample_emb_x_k为该user的第k条样本的sample_emb
user_emb_y=mean([sample_emb_y_1,sample_emb_y_2,...,sample_emb_y_k])         sample_emb_y_k为该user的第k条样本的sample_emb

通过这种对所有样本的sample_emb做mean操作来对user做embedding
item_emb_x=mean([user_emb_x_1,user_emb_x_2,...,user_emb_x_k])               user_emb_x_k为该item的第k条样本的user_emb
item_emb_y=mean([user_emb_y_1,user_emb_y_2,...,user_emb_y_k])               user_emb_y_k为该item的第k条样本的user_emb

通过这种对所有样本的use_emb做mean操作来对item做embedding
至此通过predict_category_property，item_property_list这两条信息对sample，user，item做了embedding。
得到了6*n个特征，n的大小视情况而定，这里我取了出现次数top100的property来做我的embedding，所以总共6*100个特征。 
