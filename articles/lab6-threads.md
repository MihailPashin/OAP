# Потоки

Потоки в котлине сильно урезаны, но попробуйте реализовать задачу ([тут][1] реализация на java, разобраться и переписать на котлин):

## Производители и потребители.

Есть несколько производителей и несколько потребителей, все разные потоки и работают все одновременно. Есть очередь с N элементами. Производители добавляют случайное число от 1..100, а потребители берут эти числа. Если в очереди элементов >= N производители спят, если нет элементов в очереди - потребители спят. Если элементов стало <= 0 производители просыпаются.

[1]: https://gist.github.com/GregorHorvatH/917aacbab87be892edba