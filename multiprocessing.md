<p>multiprocessing中有一個叫Queue的類，為Process間的數據交互提供一個超方便的解決方案(Queue.put(object), Queue.get())</p>

<h3>Queue類與程序罷工：</h3>
<p>如果放入Queue的Object過大（比如一個5GB的list object），很可能會導致程序卡死。在不報任何錯誤的情況下不再佔用cpu算力（還是會佔有RAM）。猜測卡死是由於Queue被大object塞爆導致的。不妨假定大object為object_A：Process A調用queue.put(object_A)在等待buffer提供更多的空間；Process B調用queue.get()在等待object_A被完整的放入Queue進而get最終結果就是Process A和Process B在此處卡死</p>
   
<p><strong>解決方法：</strong>把大object拆開，一點點往Queue放</p>

<h3>當process.join()遇到Queue類：</h3>
    <p>雖然python3官方文檔裡面，無論是創建一個pool，或是創建一個新的Process，都會在結尾的時候使用process.join()（確實有必要，不然main結束而子程序未結束的話，main會直接結束並殺死子程序），但如果main函數調用process.join()之後接著調用queue.get()的話，可以考慮去掉process.join()</p>
    <p><strong>理由：</strong>在數據較大的時候，queue可能出現問題1中卡死情況。</p>
