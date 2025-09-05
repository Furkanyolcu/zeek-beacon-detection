# zeek-beacon-detection
Unsupervised beacon detection with Zeek logs.

## Pseudocode
```bash
# 1. Read connection logs (conn.log)
# Açıklama: Zeek ile toplanan ağ bağlantı loglarını dosyadan oku.
logs = read_logs("conn.log")

# 2. Load logs into DataFrame
# Açıklama: Logları pandas DataFrame'e aktar.
df = load_into_dataframe(logs)

# 3. Sort by timestamp
# Açıklama: Kayıtları zaman damgasına göre sırala.
df = sort_by_timestamp(df)

# 4. Group by (src_ip, dst_ip, dst_port)
# Açıklama: Kayıtları kaynak IP, hedef IP ve hedef port kombinasyonlarına göre grupla.
groups = group_by_ip_port(df)

# 5. Calculate features (mean_interval, CV, count, etc.)
# Açıklama: Her grup için ortalama aralık, varyasyon katsayısı (CV), bağlantı sayısı gibi öznitelikleri hesapla.
features = calculate_features(groups)

# 6. For each group, check heuristic condition
# Açıklama: Her grup için, CV < threshold ve bağlantı sayısı > min ise heuristik olarak beacon adayı olarak işaretle.
for group in features:
    if group.cv < CV_THRESHOLD and group.count > MIN_CONNECTIONS:
        group.is_heuristic_beacon = True
    else:
        group.is_heuristic_beacon = False

# 7. ML-based detection (Isolation Forest)
# Açıklama: Tüm gruplar için Isolation Forest ile anomali skorlarını tahmin et.
ml_results = isolation_forest_detection(features)

# 8. Merge heuristic and ML results
# Açıklama: Heuristik ve ML tabanlı tespit sonuçlarını birleştir.
final_results = merge_results(features, ml_results)

# 9. Visualize and report detected beaconing groups
# Açıklama: Tespit edilen beacon gruplarını görselleştir ve raporla.
visualize_and_report(final_results)
```
