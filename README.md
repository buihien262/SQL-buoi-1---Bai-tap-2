# Bùi Thị Thu Hiền - Bai tap buoi 1 (Phan 2)
Câu 1: 
"Sort  (cost=70.17..70.27 rows=43 width=174)"
"  Sort Key: length DESC"
"  ->  Seq Scan on film  (cost=0.00..69.00 rows=43 width=174)"
"        Filter: ((rating = 'PG-13'::mpaa_rating) AND (rental_duration = 7))"

- So luong node: 2: Seq Scan on film, Sort
- Thông tin chi tiết từng node(cost(chi phí khởi động..tổng chi phí, rows(số lượng row dự kiến trả về), width(kích thước trung bình của 1 row trả về): như kết quả trong ngoặc ở trên, chú thích ngay cạnh tên node
- Node 'Seq Scan on film' duoc chay dau tien, sau do den node 'Sort'

Câu 2: 
"Sort  (cost=67.58..67.59 rows=5 width=36)"
"  Sort Key: rating DESC"
"  ->  HashAggregate  (cost=67.46..67.52 rows=5 width=36)"
"        Group Key: rating"
"        ->  Seq Scan on film  (cost=0.00..66.50 rows=191 width=6)"
"              Filter: (rental_duration = 7)"

- So luong node: 3
- Thông tin chi tiết từng node(cost(chi phí khởi động..tổng chi phí, rows(số lượng row dự kiến trả về), width(kích thước trung bình của 1 row trả về): như kết quả trong ngoặc ở trên, chú thích ngay cạnh tên node
- Thu tu chay: Seq Scan on film -> HashAggregate -> Sort

Câu 3: 
"Sort  (cost=68.03..68.04 rows=2 width=36)"
"  Sort Key: rating DESC"
"  ->  HashAggregate  (cost=67.93..68.02 rows=2 width=36)"
"        Group Key: rating"
"        Filter: (avg(length) > '115'::numeric)"
"        ->  Seq Scan on film  (cost=0.00..66.50 rows=191 width=6)"
"              Filter: (rental_duration = 7)"

- So luong node: 3
- Thông tin chi tiết từng node(cost(chi phí khởi động..tổng chi phí, rows(số lượng row dự kiến trả về), width(kích thước trung bình của 1 row trả về): như kết quả trong ngoặc ở trên, chú thích ngay cạnh tên node
- Thứ tự chạy: Seq Scan on film -> HashAggregate -> Sort

Câu 4: 
"Sort  (cost=87.71..88.75 rows=418 width=103)"
"  Sort Key: language.name DESC"
"  ->  Hash Join  (cost=1.14..69.51 rows=418 width=103)"
"        Hash Cond: (film.language_id = language.language_id)"
"        ->  Seq Scan on film  (cost=0.00..66.50 rows=418 width=21)"
"              Filter: (rating = ANY ('{R,PG-13}'::mpaa_rating[]))"
"        ->  Hash  (cost=1.06..1.06 rows=6 width=88)"
"              ->  Seq Scan on language  (cost=0.00..1.06 rows=6 width=88)"

- Số lượng node: 5
- Thông tin chi tiết từng node(cost(chi phí khởi động..tổng chi phí, rows(số lượng row dự kiến trả về), width(kích thước trung bình của 1 row trả về): như kết quả trong ngoặc ở trên, chú thích ngay cạnh tên node
- Thứ tự chạy: Seq Scan on language -> Hash -> Seq Scan on film -> Hash Join -> Sort

Câu 5: 
"Sort  (cost=71.75..71.77 rows=6 width=116)"
"  Sort Key: language.name DESC"
"  ->  HashAggregate  (cost=71.60..71.68 rows=6 width=116)"
"        Group Key: language.name"
"        ->  Hash Join  (cost=1.14..69.51 rows=418 width=86)"
"              Hash Cond: (film.language_id = language.language_id)"
"              ->  Seq Scan on film  (cost=0.00..66.50 rows=418 width=4)"
"                    Filter: (rating = ANY ('{R,PG-13}'::mpaa_rating[]))"
"              ->  Hash  (cost=1.06..1.06 rows=6 width=88)"
"                    ->  Seq Scan on language  (cost=0.00..1.06 rows=6 width=88)"

- Số lượng node: 6
- Thông tin chi tiết từng node(cost(chi phí khởi động..tổng chi phí, rows(số lượng row dự kiến trả về), width(kích thước trung bình của 1 row trả về): như kết quả trong ngoặc ở trên, chú thích ngay cạnh tên node
- Thứ tự chạy: Seq Scan on language -> Hash -> Seq Scan on film -> Hash Join -> HashAggregate -> Sort

Câu 6: 
"HashAggregate  (cost=237.62..238.90 rows=128 width=21)"
"  Group Key: actor.first_name, actor.last_name"
"  ->  Hash Join  (cost=83.00..196.65 rows=5462 width=17)"
"        Hash Cond: (film_actor.actor_id = actor.actor_id)"
"        ->  Hash Join  (cost=76.50..175.51 rows=5462 width=6)"
"              Hash Cond: (film_actor.film_id = film.film_id)"
"              ->  Seq Scan on film_actor  (cost=0.00..84.62 rows=5462 width=4)"
"              ->  Hash  (cost=64.00..64.00 rows=1000 width=4)"
"                    ->  Seq Scan on film  (cost=0.00..64.00 rows=1000 width=4)"
"        ->  Hash  (cost=4.00..4.00 rows=200 width=17)"
"              ->  Seq Scan on actor  (cost=0.00..4.00 rows=200 width=17)"

- Số lượng node: 8
- Thông tin chi tiết từng node(cost(chi phí khởi động..tổng chi phí, rows(số lượng row dự kiến trả về), width(kích thước trung bình của 1 row trả về): như kết quả trong ngoặc ở trên, chú thích ngay cạnh tên node
- Thứ tự chạy: Seq Scan on actor -> Hash -> Seq Scan on film -> Hash -> Seq Scan on film_actor -> Hash Join 
               -> Hash Join -> HashAggregate
Câu 7: 
"Hash Join  (cost=36.28..71.88 rows=23 width=196)"
"  Hash Cond: (film_category.category_id = category.category_id)"
"  ->  Nested Loop  (cost=34.92..70.45 rows=23 width=130)"
"        ->  Seq Scan on actor  (cost=0.00..4.50 rows=1 width=17)"
"              Filter: (actor_id = 1)"
"        ->  Nested Loop  (cost=34.92..65.72 rows=23 width=119)"
"              Join Filter: (film_actor.film_id = film.film_id)"
"              ->  Hash Join  (cost=34.64..53.28 rows=23 width=8)"
"                    Hash Cond: (film_category.film_id = film_actor.film_id)"
"                    ->  Seq Scan on film_category  (cost=0.00..16.00 rows=1000 width=4)"
"                    ->  Hash  (cost=34.36..34.36 rows=23 width=4)"
"                          ->  Bitmap Heap Scan on film_actor  (cost=4.46..34.36 rows=23 width=4)"
"                                Recheck Cond: (actor_id = 1)"
"                                ->  Bitmap Index Scan on film_actor_pkey  (cost=0.00..4.46 rows=23 width=0)"
"                                      Index Cond: (actor_id = 1)"
"              ->  Index Scan using film_pkey on film  (cost=0.28..0.53 rows=1 width=119)"
"                    Index Cond: (film_id = film_category.film_id)"
"  ->  Hash  (cost=1.16..1.16 rows=16 width=72)"
"        ->  Seq Scan on category  (cost=0.00..1.16 rows=16 width=72)"

- Số lượng node: 12
- Thông tin chi tiết từng node(cost(chi phí khởi động..tổng chi phí, rows(số lượng row dự kiến trả về), width(kích thước trung bình của 1 row trả về): như kết quả trong ngoặc ở trên, chú thích ngay cạnh tên node
- Thứ tự chạy: Seq Scan on category -> Hash -> Index Scan using film_pkey on film -> Bitmap Index Scan on film_actor_pkey 
               -> Bitmap Heap Scan on film_actor -> Hash -> Seq Scan on film_category -> Hash Join
               -> Nested Loop -> Seq Scan on actor ->  Nested Loop -> Hash Join
