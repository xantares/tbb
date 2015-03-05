

add_definitions (-D__TBB_BUILD=1)

include_directories (${CMAKE_SOURCE_DIR}/src/rml/include)

tbb_add_library (tbb
                concurrent_hash_map.cpp
                concurrent_queue.cpp
                concurrent_vector.cpp
                dynamic_link.cpp
                itt_notify.cpp
                cache_aligned_allocator.cpp
                pipeline.cpp
                queuing_mutex.cpp
                queuing_rw_mutex.cpp
                reader_writer_lock.cpp
                spin_rw_mutex.cpp
                x86_rtm_rw_mutex.cpp
                spin_mutex.cpp
                critical_section.cpp
                mutex.cpp
                recursive_mutex.cpp
                condition_variable.cpp
                tbb_thread.cpp
                concurrent_monitor.cpp
                semaphore.cpp
                private_server.cpp
                ${CMAKE_SOURCE_DIR}/src/rml/client/rml_tbb.cpp
                tbb_misc.cpp
                tbb_misc_ex.cpp
                task.cpp
                task_group_context.cpp
                governor.cpp
                market.cpp
                arena.cpp
                scheduler.cpp
                observer_proxy.cpp
                tbb_statistics.cpp
                tbb_main.cpp)

