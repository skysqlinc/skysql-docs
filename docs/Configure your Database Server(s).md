# Configure your Database Server(s)

Database server configuration, including system variables, is managed through the Configuration Manager.

![https://mariadb.com/docs/_images/screenshots/configuration-manager.png](https://mariadb.com/docs/_images/screenshots/configuration-manager.png)

*Configuration Manager*

https://app.skysql.com/settings/configuration-manager

## **Access to Configuration Manager**

To access the Configuration Manager interface:

1. Log in to the [Portal](https://mariadb.com/docs/skysql-dbaas/working/nr-portal/).
2. Click the "Settings" link in the main menu (left navigation in the Portal).
3. Click the "Configuration Manager" button.

## **What is configurable?**

Available configuration parameters differ by cloud database topology.

### **Enterprise Server Single Node**

For cloud databases with the Enterprise Server Single Node topology, the following Configuration Manager parameters are used to configure MariaDB Enterprise Server behavior:

**Parameter**

---

[auto_increment_increment](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/auto_increment_increment/)

---

[autocommit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/autocommit/)

---

[connect_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/connect_timeout/)

---

[cracklib_password_check](https://mariadb.com/docs/skysql-dbaas/ref/mdb/plugins/cracklib_password_check/)

---

[default_password_lifetime](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/default_password_lifetime/)

---

[default_time_zone](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/system_time_zone/)

---

[disconnect_on_expired_password](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/disconnect_on_expired_password/)

---

[div_precision_increment](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/div_precision_increment/)

---

[eq_range_index_dive_limit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/eq_range_index_dive_limit/)

---

[event_scheduler](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/event_scheduler/)

---

[expire_logs_days](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/expire_logs_days/)

---

[explicit_defaults_for_timestamp](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/explicit_defaults_for_timestamp/)

---

[idle_readonly_transaction_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/idle_readonly_transaction_timeout/)

---

[idle_transaction_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/idle_transaction_timeout/)

---

[idle_write_transaction_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/idle_write_transaction_timeout/)

---

[innodb_autoextend_increment](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_autoextend_increment/)

---

[innodb_change_buffering](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_change_buffering/)

---

[innodb_flush_log_at_trx_commit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_flush_log_at_trx_commit/)

---

[innodb_log_file_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_log_file_size/)

---

[innodb_strict_mode](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_strict_mode/)

---

[interactive_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/interactive_timeout/)

---

[local_infile](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/)

---

[log_output](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_output/)

---

[log_slow_filter](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_slow_filter/)

---

[log_slow_rate_limit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_slow_rate_limit/)

---

[log_slow_verbosity](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_slow_verbosity/)

---

[log_warnings](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_warnings/)

---

[long_query_time](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/long_query_time/)

---

[lower_case_table_names](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/lower_case_table_names/)

---

[max_allowed_packet](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/max_allowed_packet/)

---

[max_connections](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/max_connections/)

---

[max_password_errors](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/max_password_errors/)

---

[net_buffer_length](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/net_buffer_length/)

---

[net_write_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/net_write_timeout/)

---

[optimizer_search_depth](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/optimizer_search_depth/)

---

[performance_schema](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema/)

---

[performance_schema_accounts_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_accounts_size/)

---

[performance_schema_digests_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_digests_size/)

---

[performance_schema_events_stages_history_long_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_stages_history_long_size/)

---

[performance_schema_events_stages_history_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_stages_history_size/)

---

[performance_schema_events_statements_history_long_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_statements_history_long_size/)

---

[performance_schema_events_statements_history_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_statements_history_size/)

---

[performance_schema_events_waits_history_long_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_waits_history_long_size/)

---

[performance_schema_events_waits_history_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_waits_history_size/)

---

[performance_schema_hosts_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_hosts_size/)

---

[performance_schema_max_cond_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_cond_classes/)

---

[performance_schema_max_cond_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_cond_instances/)

---

[performance_schema_max_digest_length](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_digest_length/)

---

[performance_schema_max_file_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_file_classes/)

---

[performance_schema_max_file_handles](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_file_handles/)

---

[performance_schema_max_file_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_file_instances/)

---

[performance_schema_max_mutex_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_mutex_classes/)

---

[performance_schema_max_mutex_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_mutex_instances/)

---

[performance_schema_max_rwlock_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_rwlock_classes/)

---

[performance_schema_max_rwlock_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_rwlock_instances/)

---

[performance_schema_max_socket_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_socket_classes/)

---

[performance_schema_max_socket_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_socket_instances/)

---

[performance_schema_max_stage_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_stage_classes/)

---

[performance_schema_max_table_handles](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_table_handles/)

---

[performance_schema_max_table_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_table_instances/)

---

[performance_schema_max_thread_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_thread_classes/)

---

[performance_schema_max_thread_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_thread_instances/)

---

[performance_schema_session_connect_attrs_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_session_connect_attrs_size/)

---

[performance_schema_users_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_users_size/)

---

[proxy_protocol_networks](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/proxy_protocol_networks/)

---

[read_only](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/read_only/)

---

[server_audit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/plugins/SERVER_AUDIT,server_audit2.so/)

---

[server_audit_file_rotate_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/server_audit_file_rotate_size/)

---

[server_audit_logging](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/server_audit_logging/)

---

[session_track_system_variables](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/session_track_system_variables/)

---

[simple_password_check_digits](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_digits/)

---

[simple_password_check_letters_same_case](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_letters_same_case/)

---

[simple_password_check_minimal_length](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_minimal_length/)

---

[simple_password_check_other_characters](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_other_characters/)

---

[slow_query_log](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/slow_query_log/)

---

[sort_buffer_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sort_buffer_size/)

---

[sql_mode](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sql_mode/)

---

[strict_password_validation](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/strict_password_validation/)

---

[system_versioning_alter_history](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/system_versioning_alter_history/)

---

[thread_cache_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_cache_size/)

---

[thread_handling](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_handling/)

---

[thread_pool_idle_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_pool_idle_timeout/)

---

[thread_pool_max_threads](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_pool_max_threads/)

---

[thread_pool_priority](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_pool_priority/)

---

[transaction_isolation](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/tx_isolation/)

---

[wait_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/wait_timeout/)

---

### **Enterprise Server With Replica(s)**

For cloud databases with the Enterprise Server With Replica(s) topology, Configuration Manager can be used to configure MariaDB Enterprise Server behavior and MariaDB MaxScale behavior.

The following Configuration Manager parameters are used to configure MariaDB Enterprise Server behavior:

**Parameter**

---

[auto_increment_increment](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/auto_increment_increment/)

---

[autocommit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/autocommit/)

---

[binlog_commit_wait_count](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/binlog_commit_wait_count/)

---

[binlog_commit_wait_usec](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/binlog_commit_wait_usec/)

---

[connect_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/connect_timeout/)

---

[cracklib_password_check](https://mariadb.com/docs/skysql-dbaas/ref/mdb/plugins/cracklib_password_check/)

---

[default_password_lifetime](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/default_password_lifetime/)

---

[default_time_zone](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/system_time_zone/)

---

[disconnect_on_expired_password](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/disconnect_on_expired_password/)

---

[div_precision_increment](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/div_precision_increment/)

---

[eq_range_index_dive_limit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/eq_range_index_dive_limit/)

---

[event_scheduler](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/event_scheduler/)

---

[expire_logs_days](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/expire_logs_days/)

---

[explicit_defaults_for_timestamp](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/explicit_defaults_for_timestamp/)

---

[gtid_strict_mode](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/gtid_strict_mode/)

---

[idle_readonly_transaction_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/idle_readonly_transaction_timeout/)

---

[idle_transaction_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/idle_transaction_timeout/)

---

[idle_write_transaction_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/idle_write_transaction_timeout/)

---

[innodb_autoextend_increment](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_autoextend_increment/)

---

[innodb_change_buffering](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_change_buffering/)

---

[innodb_flush_log_at_trx_commit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_flush_log_at_trx_commit/)

---

[innodb_log_file_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_log_file_size/)

---

[innodb_strict_mode](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/innodb_strict_mode/)

---

[interactive_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/interactive_timeout/)

---

[local_infile](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/local_infile/)

---

[log_output](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_output/)

---

[log_slow_filter](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_slow_filter/)

---

[log_slow_rate_limit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_slow_rate_limit/)

---

[log_slow_verbosity](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_slow_verbosity/)

---

[log_warnings](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/log_warnings/)

---

[long_query_time](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/long_query_time/)

---

[lower_case_table_names](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/lower_case_table_names/)

---

[max_allowed_packet](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/max_allowed_packet/)

---

[max_connections](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/max_connections/)

---

[max_password_errors](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/max_password_errors/)

---

[net_buffer_length](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/net_buffer_length/)

---

[net_write_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/net_write_timeout/)

---

[optimizer_search_depth](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/optimizer_search_depth/)

---

[performance_schema](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema/)

---

[performance_schema_accounts_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_accounts_size/)

---

[performance_schema_digests_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_digests_size/)

---

[performance_schema_events_stages_history_long_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_stages_history_long_size/)

---

[performance_schema_events_stages_history_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_stages_history_size/)

---

[performance_schema_events_statements_history_long_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_statements_history_long_size/)

---

[performance_schema_events_statements_history_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_statements_history_size/)

---

[performance_schema_events_waits_history_long_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_waits_history_long_size/)

---

[performance_schema_events_waits_history_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_events_waits_history_size/)

---

[performance_schema_hosts_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_hosts_size/)

---

[performance_schema_max_cond_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_cond_classes/)

---

[performance_schema_max_cond_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_cond_instances/)

---

[performance_schema_max_digest_length](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_digest_length/)

---

[performance_schema_max_file_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_file_classes/)

---

[performance_schema_max_file_handles](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_file_handles/)

---

[performance_schema_max_file_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_file_instances/)

---

[performance_schema_max_mutex_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_mutex_classes/)

---

[performance_schema_max_mutex_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_mutex_instances/)

---

[performance_schema_max_rwlock_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_rwlock_classes/)

---

[performance_schema_max_rwlock_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_rwlock_instances/)

---

[performance_schema_max_socket_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_socket_classes/)

---

[performance_schema_max_socket_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_socket_instances/)

---

[performance_schema_max_stage_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_stage_classes/)

---

[performance_schema_max_table_handles](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_table_handles/)

---

[performance_schema_max_table_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_table_instances/)

---

[performance_schema_max_thread_classes](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_thread_classes/)

---

[performance_schema_max_thread_instances](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_max_thread_instances/)

---

[performance_schema_session_connect_attrs_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_session_connect_attrs_size/)

---

[performance_schema_users_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/performance_schema_users_size/)

---

[proxy_protocol_networks](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/proxy_protocol_networks/)

---

[read_only](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/read_only/)

---

[rpl_semi_sync_master_enabled](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/rpl_semi_sync_master_enabled/)

---

[rpl_semi_sync_master_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/rpl_semi_sync_master_timeout/)

---

[rpl_semi_sync_master_wait_no_slave](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/rpl_semi_sync_master_wait_no_slave/)

---

[rpl_semi_sync_master_wait_point](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/rpl_semi_sync_master_wait_point/)

---

[rpl_semi_sync_slave_delay_master](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/rpl_semi_sync_slave_delay_master/)

---

[rpl_semi_sync_slave_enabled](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/rpl_semi_sync_slave_enabled/)

---

[rpl_semi_sync_slave_kill_conn_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/rpl_semi_sync_slave_kill_conn_timeout/)

---

[server_audit](https://mariadb.com/docs/skysql-dbaas/ref/mdb/plugins/SERVER_AUDIT,server_audit2.so/)

---

[server_audit_file_rotate_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/server_audit_file_rotate_size/)

---

[server_audit_logging](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/server_audit_logging/)

---

[session_track_system_variables](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/session_track_system_variables/)

---

[simple_password_check_digits](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_digits/)

---

[simple_password_check_letters_same_case](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_letters_same_case/)

---

[simple_password_check_minimal_length](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_minimal_length/)

---

[simple_password_check_other_characters](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/simple_password_check_other_characters/)

---

[slave_compressed_protocol](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/slave_compressed_protocol/)

---

[slave_parallel_mode](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/slave_parallel_mode/)

---

[slave_parallel_threads](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/slave_parallel_threads/)

---

[slave_parallel_workers](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/slave_parallel_workers/)

---

[slow_query_log](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/slow_query_log/)

---

[sort_buffer_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sort_buffer_size/)

---

[sql_mode](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sql_mode/)

---

[strict_password_validation](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/strict_password_validation/)

---

[sync_binlog](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sync_binlog/)

---

[sync_master_info](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sync_master_info/)

---

[sync_relay_log](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sync_relay_log/)

---

[sync_relay_log_info](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/sync_relay_log_info/)

---

[system_versioning_alter_history](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/system_versioning_alter_history/)

---

[thread_cache_size](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_cache_size/)

---

[thread_handling](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_handling/)

---

[thread_pool_idle_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_pool_idle_timeout/)

---

[thread_pool_max_threads](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_pool_max_threads/)

---

[thread_pool_priority](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/thread_pool_priority/)

---

[transaction_isolation](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/tx_isolation/)

---

[wait_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mdb/system-variables/wait_timeout/)

---

## Maxscale configuration

The following Configuration Manager parameters are used to configure MariaDB MaxScale behavior:

**Parameter**

---

[causal_reads](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/causal_reads/)

---

[causal_reads_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/causal_reads_timeout/)

---

[delayed_retry](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/delayed_retry/)

---

[delayed_retry_timeout](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/delayed_retry_timeout/)

---

[failcount](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/failcount/)

---

[master_accept_reads](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/master_accept_reads,Router.readwritesplit/)

---

[master_reconnection](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/master_reconnection/)

---

[max_slave_connections](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/max_slave_connections/)

---

[max_slave_replication_lag](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/max_slave_replication_lag/)

---

[monitor_interval](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/monitor_interval,Monitor.mariadbmon/)

---

[slave_selection_criteria](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/slave_selection_criteria/)

---

[strict_multi_stmt](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/strict_multi_stmt/)

---

[strict_sp_calls](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/strict_sp_calls/)

---

[transaction_replay](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/transaction_replay/)

---

[transaction_replay_attempts](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/transaction_replay_attempts/)

---

[transaction_replay_max_size](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/transaction_replay_max_size/)

---

[transaction_replay_retry_on_deadlock](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/transaction_replay_retry_on_deadlock/)

---

[use_sql_variables_in](https://mariadb.com/docs/skysql-dbaas/ref/mxs/module-parameters/use_sql_variables_in/)

---