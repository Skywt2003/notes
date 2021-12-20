# 一些坑点

## C++ 空指针问题

二路归并中 `if (head2==NULL || head1->data < head2->data) p->data=head1->data, head1 = head1->next;` 写法是有问题的，`head1` 可能为 `NULL`。应该写为  `if (head2==NULL || (head1!=NULL && head1->data < head2->data) p->data=head1->data, head1 = head1->next;` 



